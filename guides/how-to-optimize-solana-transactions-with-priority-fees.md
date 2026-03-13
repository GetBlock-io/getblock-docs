---
description: >-
  Learn how to land your Solana transaction faster using priority fee and
  GetBlock
---

# How To Optimize Solana Transactions with Priority Fees

Solana processes thousands of transactions per second, but during periods of high network activity, your transactions may compete for block space. Priority fees let you bid for faster inclusion in the validator's queue, ensuring your time-sensitive transactions reach the network quickly.

_In this guide, you'll learn about priority fees, when you should use them, how to calculate compute uint(CU), and how to create one for yourself._&#x20;

### What Are Priority Fees?

Priority fees are optional fees you add to a Solana transaction to increase its scheduling priority. Think of them as a tip to validators—transactions with higher priority fees get processed before those with lower fees or no fees at all.

The formula is simple:

```bash
Priority Fee = Compute Units × Compute Unit Price
```

Where:

* **Compute Units (CU)** measure the computational resources your transaction consumes
* **Compute Unit Price** is how much you're willing to pay per CU, measured in microLamports

Quick conversion:

* 1 SOL = 1,000,000,000 lamports
* 1 lamport = 1,000,000 microLamports
* Therefore: 1 microLamport = 0.000000000001 SOL

### When Should You Use Priority Fees?

Priority fees aren't always necessary. Here's when they make sense:

| Scenario                                       | Recommendation              |
| ---------------------------------------------- | --------------------------- |
| Simple wallet transfers during normal activity | Skip or use minimal fees    |
| DeFi swaps and trades                          | Medium priority recommended |
| NFT mints during launch                        | High to urgent priority     |
| Arbitrage opportunities                        | Extreme priority            |
| Network congestion detected                    | Increase from baseline      |
| Time-sensitive operations                      | High priority minimum       |

{% hint style="info" %}
During normal network conditions, transactions land fine without priority fees. Only add them when speed matters or when the network is congested.
{% endhint %}

### Understanding Compute Units

Every Solana transaction consumes compute units. The more complex your transaction, the more CUs it uses.

Typical CU usage:

* Simple SOL transfer: \~450 CU
* Token transfer (SPL): \~20,000-30,000 CU
* DEX swap: \~100,000-200,000 CU
* Complex DeFi operations: \~200,000-400,000 CU

{% hint style="info" %}
You're charged based on the compute unit **limit** you set, not what you actually use. If you set a limit of 200,000 CU but only use 50,000, you still pay for 200,000. This is why simulation matters.
{% endhint %}

Default behavior:

* Without `SetComputeUnitLimit`: Solana defaults to 200,000 CU per instruction
* Without `SetComputeUnitPrice`: No priority fee is charged (only base fee)

### Prerequisites

You must have the following:

1. [Node.js v23+](https://nodejs.org/en/download) is installed on your laptop
2. Solana Wallet, e.g., [Phantom](https://phantom.com/), [Solflare](https://www.solflare.com/), [MetaMask](https://metamask.io/), etc
3. Solana Devnet RPC URL (Optional: [GetBlock Solana RPC URL](https://account.getblock.io/))
4. A package manager installed on your laptop(Preferrably npm)

{% hint style="info" %}
Devnet RPC URL is used for this guide. If you want to interact with the Mainnet, then make use of [GetBlock Solana RPC URL](https://account.getblock.io/)
{% endhint %}

## Project Setup

{% stepper %}
{% step %}
Create a new project directory

{% code overflow="wrap" %}
```bash
mkdir priority-fees
cd priority-fees
```
{% endcode %}
{% endstep %}

{% step %}
Install the dependencies:

```bash
npm install @solana/web3.js dotenv base58
```

<details>

<summary>Package Description</summary>

| Package         | Description                                                       |
| --------------- | ----------------------------------------------------------------- |
| @solana/web3.js | Standard library for talking to the Solana blockchain and RPCs    |
| dotenv          | Loads your `.env` file so secrets are not hardcoded in the source |
| base58          | Decodes base58 private keys into a format the code can use        |

</details>
{% endstep %}

{% step %}
Create a `.env` file and add the following details:

{% code overflow="wrap" %}
```typescript
RPC_URL=https://api.devnet.solana.com //GETBLOCK_API_KEY=your_getblock_api
PRIVATE_KEY=your_wallet_private_key
RECIPIENT_WALLET=the_receiver_wallet
```
{% endcode %}
{% endstep %}

{% step %}
Create `index.ts` file, this is where you will be writing your script
{% endstep %}

{% step %}
Import your dependencies and load the `.env` variables:

{% code overflow="wrap" %}
```typescript
import {
  Connection,
  Keypair,
  Transaction,
  TransactionMessage,
  VersionedTransaction,
  SystemProgram,
  PublicKey,
  ComputeBudgetProgram,
  sendAndConfirmTransact
  LAMPORTS_PER_SOL,
  TransactionInstruction
} from "@solana/web3.js"
import "dotenv/config";
import bs58 from "bs58";

const SOLANA_RPC = process.env.RPC_URL; //replace with GETBLOCK_API_KEY if you are interacting with Mainnet
const PRIVATE_KEY = process.env.PRIVATE_KEY;
const RECIPIENT_WALLET = process.env.RECIPIENT_WALLET;
```
{% endcode %}
{% endstep %}

{% step %}
Build the Transfer Instruction

{% code overflow="wrap" %}
```typescript
async function sendWithPriorityFee(
  wallet: Keypair,
  recipient: PublicKey,
  amountSol: number,
): Promise<string> {
  const connection = new Connection(SOLANA_RPC!);

  // 1. Create the transfer instruction
  const transferIx = SystemProgram.transfer({
    fromPubkey: wallet.publicKey,
    toPubkey: recipient,
    lamports: amountSol * LAMPORTS_PER_SOL,
  });
```
{% endcode %}

This creates the instruction to move SOL. `LAMPORTS_PER_SOL` (1,000,000,000) converts from SOL to the smallest unit (lamports).&#x20;
{% endstep %}

{% step %}
&#x20;Simulate to Estimate Compute Units

{% code overflow="wrap" %}
```typescript
 const { blockhash: simBlockhash } = await connection.getLatestBlockhash();
 const simMessage = new TransactionMessage({
   payerKey: wallet.publicKey,
   recentBlockhash: simBlockhash,
   instructions: [transferIx],
 }).compileToV0Message();
 const simTx = new VersionedTransaction(simMessage);
 const simulation = await connection.simulateTransaction(simTx);
 // Add 600 CU overhead to account for the two ComputeBudget instructions
 // (each ~150 CU), which are not included in the simulation
 const estimatedCU = Math.max(
   Math.ceil((simulation.value.unitsConsumed || 200_000) * 1.1) + 600,
   600,
 );
```
{% endcode %}

This:

* Wraps the transfer instruction in a versioned transaction (the modern Solana format) for simulation.
* Gets a recent blockhash — required to build a valid transaction even for simulation.
* Dry-runs the transaction against the network without actually submitting it. Returns how many compute units it would use.
* Takes the simulated CU count, adds a 10% buffer, then adds 600 extra to account for the two `ComputeBudget` instructions that will be in the final transaction but weren't in the simulation. Falls back to 200,000 if the simulation returned nothing.
{% endstep %}

{% step %}
Get a Dynamic Priority Fee and calculate the total cost

{% code overflow="wrap" %}
```typescript
const recentFees = await connection.getRecentPrioritizationFees();
const sortedFees = recentFees
  .map((f) => f.prioritizationFee)
  .filter((f) => f > 0)
  .sort((a, b) => a - b);
const dynamicFee =
  sortedFees.length > 0
    ? sortedFees[Math.floor(sortedFees.length * 0.75)]
    : 10_000;
console.log(`Priority fee: ${dynamicFee} microLamports/CU`);

const totalPriorityFee = Math.ceil((estimatedCU * dynamicFee) / 1_000_000);
console.log(
  `Total priority fee: ${totalPriorityFee} lamports (${totalPriorityFee / LAMPORTS_PER_SOL} SOL)`,
);
```
{% endcode %}

This helps gain insight into what was recently paid for priority fees and calculate what you paid, rather than guessing, which can prevent wastage or setting the fee too low.
{% endstep %}

{% step %}
Build the final transaction

{% code overflow="wrap" %}
```typescript
  const { blockhash } = await connection.getLatestBlockhash("confirmed");
  const finalTx = new Transaction();
  finalTx.add(ComputeBudgetProgram.setComputeUnitLimit({ units: estimatedCU }));
  finalTx.add(
    ComputeBudgetProgram.setComputeUnitPrice({ microLamports: dynamicFee }),
  );
  finalTx.add(transferIx);
  finalTx.recentBlockhash = blockhash;
  finalTx.feePayer = wallet.publicKey;

  finalTx.sign(wallet);
  const signature = await sendAndConfirmTransaction(
    connection,
    finalTx,
    [wallet],
    { skipPreflight: false, preflightCommitment: "confirmed" },
  );
  console.log(
    `Transaction confirmed: https://solscan.io/tx/${signature}?cluster=devnet`,
  );
  return signature;
}

const privateKeyBytes = bs58.decode(PRIVATE_KEY!);
const wallet = Keypair.fromSecretKey(privateKeyBytes);
const recipient = new PublicKey(RECIPIENT_WALLET!);
const amountSol = 0.001; // Amount in SOL to send
sendWithPriorityFee(wallet, recipient, amountSol).catch(console.error);
```
{% endcode %}

This:&#x20;

* Assembles 3 instructions: CU limit → priority fee → SOL transfer, then attaches blockhash and fee payer.
* Signs with the private key, submits, waits for confirmation, and returns the signature.
* Decodes private key, creates wallet keypair, parses recipient address, calls function with `0.001 SOL`.

<details>

<summary>Complete Working Example</summary>

Here's a full example that puts everything together:

{% code title="index.ts" overflow="wrap" %}
```typescript
import {
  Connection,
  Keypair,
  Transaction,
  TransactionMessage,
  VersionedTransaction,
  SystemProgram,
  PublicKey,
  ComputeBudgetProgram,
  sendAndConfirmTransaction,
  LAMPORTS_PER_SOL,
  TransactionInstruction,
} from "@solana/web3.js";
import "dotenv/config";
import bs58 from "bs58";

const SOLANA_RPC = process.env.GETBLOCK_API_KEY;
const PRIVATE_KEY = process.env.PRIVATE_KEY;
const RECIPIENT_WALLET = process.env.RECIPIENT_WALLET;

if (!SOLANA_RPC || !PRIVATE_KEY || !RECIPIENT_WALLET) {
  throw new Error("Missing required environment variables: GETBLOCK_API_KEY, PRIVATE_KEY, or RECIPIENT_WALLET");
}

async function sendWithPriorityFee(
  wallet: Keypair,
  recipient: PublicKey,
  amountSol: number,
): Promise<string> {
  const connection = new Connection(SOLANA_RPC!);

  // 1. Create the transfer instruction
  const transferIx = SystemProgram.transfer({
    fromPubkey: wallet.publicKey,
    toPubkey: recipient,
    lamports: amountSol * LAMPORTS_PER_SOL,
  });

  // 2. Simulate to estimate compute units
  const { blockhash: simBlockhash } = await connection.getLatestBlockhash();
  const simMessage = new TransactionMessage({
    payerKey: wallet.publicKey,
    recentBlockhash: simBlockhash,
    instructions: [transferIx],
  }).compileToV0Message();
  const simTx = new VersionedTransaction(simMessage);

  const simulation = await connection.simulateTransaction(simTx);
  // Add 600 CU overhead to account for the two ComputeBudget instructions
  // (each ~150 CU), which are not included in the simulation
  const estimatedCU = Math.max(
    Math.ceil((simulation.value.unitsConsumed || 200_000) * 1.1) + 600,
    600,
  );

  console.log(`Estimated CU: ${estimatedCU}`);

  // 3. Get dynamic priority fee from network
  const recentFees = await connection.getRecentPrioritizationFees();
  const sortedFees = recentFees
    .map((f) => f.prioritizationFee)
    .filter((f) => f > 0)
    .sort((a, b) => a - b);

  const dynamicFee =
    sortedFees.length > 0
      ? sortedFees[Math.floor(sortedFees.length * 0.75)]
      : 10_000;

  console.log(`Priority fee: ${dynamicFee} microLamports/CU`);

  // 4. Calculate total cost
  const totalPriorityFee = Math.ceil((estimatedCU * dynamicFee) / 1_000_000);
  console.log(
    `Total priority fee: ${totalPriorityFee} lamports (${totalPriorityFee / LAMPORTS_PER_SOL} SOL)`,
  );

  // 5. Build final transaction
  const { blockhash } = await connection.getLatestBlockhash("confirmed");

  const finalTx = new Transaction();
  finalTx.add(ComputeBudgetProgram.setComputeUnitLimit({ units: estimatedCU }));
  finalTx.add(
    ComputeBudgetProgram.setComputeUnitPrice({ microLamports: dynamicFee }),
  );
  finalTx.add(transferIx);
  finalTx.recentBlockhash = blockhash;
  finalTx.feePayer = wallet.publicKey;

  // 6. Sign and send
  finalTx.sign(wallet);

  const signature = await sendAndConfirmTransaction(
    connection,
    finalTx,
    [wallet],
    { skipPreflight: false, preflightCommitment: "confirmed" },
  );

  console.log(
    `Transaction confirmed: https://solscan.io/tx/${signature}?cluster=devnet`,
  );
  return signature;
}

// Main entry point
const privateKeyBytes = bs58.decode(PRIVATE_KEY!);
const wallet = Keypair.fromSecretKey(privateKeyBytes);
const recipient = new PublicKey(RECIPIENT_WALLET!);
const amountSol = 0.001; // Amount in SOL to send

sendWithPriorityFee(wallet, recipient, amountSol).catch(console.error);

```
{% endcode %}

</details>
{% endstep %}

{% step %}
Run the code using this command

{% code overflow="wrap" %}
```bash
npx ts-node index.ts
```
{% endcode %}
{% endstep %}

{% step %}
Result

You will get a response like this:\
This should show the estimated CU, the total priority fee used, and the transaction hash

{% code overflow="wrap" %}
```bash
Estimated CU: 765
Priority fee: 10000 microLamports/CU
Total priority fee: 8 lamports (8e-9 SOL)
Transaction confirmed: https://solscan.io/tx/2xaGFBomVphZWDBwiT4KeyqwHrDkgsoLdhFawbXotAam43yziCwUdSqQCGiXtoHF2h8gAAEnZ4GjNi3tQ8QX3RSp?cluster=devnet
```
{% endcode %}
{% endstep %}
{% endstepper %}

## Best Practices

1. Never hardcode compute unit limits. Simulation catches errors before they cost you fees:

```typescript
// Good: Simulate and use actual consumption + buffer
const simulation = await connection.simulateTransaction(tx);
const estimatedCU = Math.ceil(simulation.value.unitsConsumed * 1.1);

// Bad: Hardcoded value wastes money or fails
const estimatedCU = 1_400_000; // Don't do this
```

2. Network conditions change. Fetch recent fees instead of using static values:

```typescript
// Good: Adapt to current network conditions
const fees = await connection.getRecentPrioritizationFees();
const dynamicFee = calculatePercentile(fees, 75);

// Bad: Static fee may be too low or waste money
const staticFee = 50_000; // Don't do this
```

### Conclusion

Priority fees are a powerful tool for ensuring your Solana transactions reach their destination quickly during network congestion. The key steps are:

1. Build your instructions - Create the core transaction logic
2. Simulate - Get actual compute unit requirements
3. Fetch network fees - Understand current market rates
4. Add compute budget - Set limit and price (in that order, before main instructions)
5. Send - Sign and submit with appropriate commitment level

#### Additional Resources

* [Solana Core Fees](https://solana.com/docs/core/fees)
* [GetBlock Dashboard](https://account.getblock.io)
* [Solana Devnet Explorer](https://solscan.io/?cluster=devnet)
* [Priority fee Full Code repository](https://github.com/GetBlock-io/guides/blob/main/priority-fees)
* [Solana Compute Budget](https://solana.com/docs/core/fees/compute-budget)
