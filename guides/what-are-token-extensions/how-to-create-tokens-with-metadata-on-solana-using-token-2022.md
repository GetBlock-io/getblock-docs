---
description: >-
  Create Solana SPL tokens with metadata using Token-2022 program — developer
  guide
---

# How to Create Tokens With Metadata On Solana Using Token-2022

Now that you have learnt about [token extension](./), in this guide, you will be learning how to create a simple token with metadata extension

### Prerequistics

Before you begin, ensure you have the following:

1. Node installed on your PC, preferably v20+
2. A package manager such as npm or yarn
3. Solana Wallet, e.g., [Phantom](https://phantom.com/), [Solfare](https://www.solflare.com/), or [Metamask](https://metamask.io/)
4. Solana Access token from [GetBlock](https://account.getblock.io/)&#x20;
5. Basic knowledge of JavaScript and Typescript
6. Basic understanding of Solana (wallets, transactions, lamports)

### Set up your environment&#x20;

{% stepper %}
{% step %}
Create a new project directory:

<pre class="language-bash"><code class="lang-bash">mkdir token-2022-tutorial
<strong>cd token-2022-tutorial
</strong>npm init -y
</code></pre>
{% endstep %}

{% step %}
Install required dependencies:

```bash
npm install @solana/web3.js @solana/spl-token @solana/spl-token-metadata dotenv
```
{% endstep %}

{% step %}
Create a `.env` file and add the following:

{% code title=".env" overflow="wrap" %}
```js
RPC_URL=https://api.devnet.solana.com //or mainnet URL
PRIVATE_KEY=your-wallet-private-key
```
{% endcode %}
{% endstep %}
{% endstepper %}

### Configuration File

Create `config.js` file and add the following:

{% code title="config.js" overflow="wrap" %}
```js
import { Connection, Keypair } from "@solana/web3.js";
import bs58 from "bs58";

import 'dotenv/config'

// Replace with your GetBlock endpoint
const GETBLOCK_RPC_URL = process.env.RPC_URL;

// Create connection to Solana via GetBlock
const connection = new Connection(GETBLOCK_RPC_URL, "confirmed");

// Load keypair from private key in .env 
const payer = Keypair.fromSecretKey(bs58.decode(process.env.PRIVATE_KEY));

export { connection, payer};
```
{% endcode %}

### Create a Token with Metadata Extension

{% stepper %}
{% step %}
Create a new file `index.js` . This will serve as your working file
{% endstep %}

{% step %}
Import all the dependencies:

{% code overflow="wrap" %}
```js
import { 
Keypair, 
SystemProgram, 
Transaction, 
sendAndConfirmTransaction } 
from "@solana/web3.js";

import { TOKEN_2022_PROGRAM_ID, 
ExtensionType, 
createInitializeMintInstruction, 
createInitializeMetadataPointerInstruction, 
getMintLen, 
TYPE_SIZE, 
LENGTH_SIZE } 
from "@solana/spl-token";

import { createInitializeInstruction, pack } from "@solana/spl-token-metadata";
import { connection, payer } from "./config.js";
```
{% endcode %}
{% endstep %}

{% step %}
Generate an account for the mint address

{% code overflow="wrap" %}
```js
async function createTokenWithMetadata() {
  console.log("=== Creating Token with Metadata Extension ===\n");
  console.log("Payer address:", payer.publicKey.toBase58());
  const mintKeypair = Keypair.generate();
  const mint = mintKeypair.publicKey;
  console.log("\nMint address:", mint.toBase58());
```
{% endcode %}
{% endstep %}

{% step %}
Define the token metadata:

{% code overflow="wrap" %}
```js
 const metadata = {
   updateAuthority: payer.publicKey,
   mint: mint,
   name: "GetBlock Tutorial Token",
   symbol: "GBT",
   uri: "https://getblock.io/",
   additionalMetadata: [["description", "A test token created with GetBlock"]],
 };
```
{% endcode %}
{% endstep %}

{% step %}
Calculate the metadata space and rent needed for the mint account:

{% code overflow="wrap" %}
```js
const decimals = 9;
const mintLen = getMintLen([ExtensionType.MetadataPointer]);
const metadataLen =
  TYPE_SIZE + // 2 bytes for type
  LENGTH_SIZE + // 2 bytes for length
  pack(metadata).length; // Actual metadata size
const lamports = await connection.getMinimumBalanceForRentExemp
  mintLen + metadataLen,
);
```
{% endcode %}

This ensures our token mint account has sufficient space for the extensions you use or else the metadata initialization would fail with an `insufficient lamports` error.
{% endstep %}

{% step %}
Define the structure of the transaction:

{% code overflow="wrap" %}
```js
const transaction = new Transaction().add(
  SystemProgram.createAccount({
    fromPubkey: payer.publicKey,
    newAccountPubkey: mint,
    space: mintLen, // ← Just the mint size
    lamports: lamports, // ← But funded for full size
    programId: TOKEN_2022_PROGRAM_ID,
  }),
  createInitializeMetadataPointerInstruction(
    mint, // mint
    payer.publicKey, // authority
    mint, // metadata address (pointing to itself)
    TOKEN_2022_PROGRAM_ID,
  ),
  createInitializeMintInstruction(
    mint, // mint
    decimals, // decimals
    payer.publicKey, // mint authority
    null, // freeze authority (optional)
    TOKEN_2022_PROGRAM_ID,
  ),
  createInitializeInstruction({
    programId: TOKEN_2022_PROGRAM_ID,
    mint: mint,
    metadata: mint,
    name: metadata.name,
    symbol: metadata.symbol,
    uri: metadata.uri,
    mintAuthority: payer.publicKey,
    updateAuthority: payer.publicKey,
  }),
);
console.log("\nSending transaction...");
const signature = await sendAndConfirmTransaction(
  connection,
  transaction,
  [payer, mintKeypair], // Signers
  { commitment: "confirmed" },
);
console.log("\Token created successfully!");
console.log("Transaction signature:", signature);
console.log("Mint address:", mint.toBase58());
console.log("\nToken details:");
console.log("  Name:", metadata.name);
console.log("  Symbol:", metadata.symbol);
console.log("  Decimals:", decimals);
console.log("  URI:", metadata.uri);
return mint;
}
ceateTokenWithMetadata()
.then(() => process.exit(0))
.catch((error) => {
  console.error("Error:", error);
  process.exit(1);
});
```
{% endcode %}

This creates the mint address, initializes the mint, metadata, points to the mint itself as the metadata account, and sends the transaction.

<details>

<summary>Complete code</summary>

{% code title="index.js" overflow="wrap" %}
```js
import { Keypair, SystemProgram, Transaction, sendAndConfirmTransaction } from "@solana/web3.js";
import { TOKEN_2022_PROGRAM_ID, ExtensionType, createInitializeMintInstruction, createInitializeMetadataPointerInstruction, getMintLen, TYPE_SIZE, LENGTH_SIZE } from "@solana/spl-token";
import { createInitializeInstruction, pack } from "@solana/spl-token-metadata";
import { connection, payer } from "./config.js";
async function createTokenWithMetadata() {
  console.log("=== Creating Token with Metadata Extension ===\n");
  console.log("Payer address:", payer.publicKey.toBase58());
  const mintKeypair = Keypair.generate();
  const mint = mintKeypair.publicKey;
  console.log("\nMint address:", mint.toBase58());
  const metadata = {
    updateAuthority: payer.publicKey,
    mint: mint,
    name: "GetBlock Tutorial Token",
    symbol: "GBT",
    uri: "https://getblock.io/",
    additionalMetadata: [["description", "A test token created with GetBlock"]],
  };
  const decimals = 9;
  const mintLen = getMintLen([ExtensionType.MetadataPointer]);
  const metadataLen =
    TYPE_SIZE + // 2 bytes for type
    LENGTH_SIZE + // 2 bytes for length
    pack(metadata).length; // Actual metadata size
  const lamports = await connection.getMinimumBalanceForRentExemption(
    mintLen + metadataLen,
  );
  const transaction = new Transaction().add(
    SystemProgram.createAccount({
      fromPubkey: payer.publicKey,
      newAccountPubkey: mint,
      space: mintLen, // ← Just the mint size
      lamports: lamports, // ← But funded for full size
      programId: TOKEN_2022_PROGRAM_ID,
    }),
    createInitializeMetadataPointerInstruction(
      mint, // mint
      payer.publicKey, // authority
      mint, // metadata address (pointing to itself)
      TOKEN_2022_PROGRAM_ID,
    ),
    createInitializeMintInstruction(
      mint, // mint
      decimals, // decimals
      payer.publicKey, // mint authority
      null, // freeze authority (optional)
      TOKEN_2022_PROGRAM_ID,
    ),
    createInitializeInstruction({
      programId: TOKEN_2022_PROGRAM_ID,
      mint: mint,
      metadata: mint,
      name: metadata.name,
      symbol: metadata.symbol,
      uri: metadata.uri,
      mintAuthority: payer.publicKey,
      updateAuthority: payer.publicKey,
    }),
  );
  console.log("\nSending transaction...");
  const signature = await sendAndConfirmTransaction(
    connection,
    transaction,
    [payer, mintKeypair], // Signers
    { commitment: "confirmed" },
  );

  console.log("\Token created successfully!");
  console.log("Transaction signature:", signature);
  console.log("Mint address:", mint.toBase58());
  console.log("\nToken details:");
  console.log("  Name:", metadata.name);
  console.log("  Symbol:", metadata.symbol);
  console.log("  Decimals:", decimals);
  console.log("  URI:", metadata.uri);

  return mint;
}
createTokenWithMetadata()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error("Error:", error);
    process.exit(1);
  });

```
{% endcode %}

</details>
{% endstep %}

{% step %}
Run the code using this command:

{% code overflow="wrap" %}
```bash
node index.js
```
{% endcode %}
{% endstep %}

{% step %}
Result:&#x20;

{% code overflow="wrap" %}
```bash
=== Creating Token with Metadata Extension ===

Payer address: 5C7dUe5ZDWBCYQ5eqtGPRyz7CQYk2aK4FhZTDYQeji7r

Mint address: BVwyGVpUNUL7eYVqbMtzgbinTiUgd3tNXsgxfxoAQaWe

Sending transaction...

Token created successfully!
Transaction signature: N9DHUYdNRUc9RC2H6LMtiSPpY2UW1aVmFZsK7b8KG3xpTUWiVDrj79EkxHTh74T5QmbMR3CbprYif3jNveAnQBJ
Mint address: BVwyGVpUNUL7eYVqbMtzgbinTiUgd3tNXsgxfxoAQaWe

Token details:
  Name: GetBlock Tutorial Token
  Symbol: GBT
  Decimals: 9
  URI: https://getblock.io/
```
{% endcode %}

Verify on the [Solscan](https://solscan.io/token/BVwyGVpUNUL7eYVqbMtzgbinTiUgd3tNXsgxfxoAQaWe?cluster=devnet):&#x20;

<figure><img src="../../.gitbook/assets/Screenshot 2026-03-22 at 10.14.03 PM.png" alt="Solscan verification"><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}

### Conclusion

In this guide, you have learnt how to create a token with metadata extension. This guide also walks you through generating a Mint account for your token.&#x20;

{% hint style="info" %}
This guide uses Solana Devnet. If you want to make use of Mainnet, go to your [account dashboard ](https://account.getblock.io/)and get a Solana mainnet access token.&#x20;
{% endhint %}

#### Additional Resources

1. [Token Metadata Interface](https://github.com/solana-program/token-metadata/tree/main)
2. [Solana Token](https://solana.com/docs/tokens)
3. [What are Token Extensions](./)
4. [Solana program](https://github.com/solana-program)
5. [GetBlock Account Dashboard](https://account.getblock.io/)
6. [Create Token With Metadata Repo](https://github.com/GetBlock-io/guides/blob/main/token-2022-tutorial/create-token-with-metadata.js)
