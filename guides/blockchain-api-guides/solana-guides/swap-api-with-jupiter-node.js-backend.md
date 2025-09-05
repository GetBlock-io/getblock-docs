---
description: >-
  Step-by-step Node.js integration with Jupiter swap and GetBlock Solana RPC.
  Implement Jupiter quote and swap flows server-side
---

# Swap API with Jupiter – Node.js backend

Build a simple backend endpoint that requests swap routes from Jupiter (quote → swap), signs the returned versioned transaction, simulates it, then submits it through the GetBlock’s Solana RPC API.

***

### Prerequisites and dependencies

* Node.js (v16+)&#x20;
* A GetBlock account with a Solana endpoint (create an account on GetBlock.io and [generate an access token](https://docs.getblock.io/guides/endpoint-setup/creating-node-endpoints))
* A Jupiter API key if your usage or the Jupiter host requires it

Create a project and install dependencies:

```bash
mkdir solana-swap-api && cd solana-swap-api
npm init -y
npm install @solana/web3.js axios dotenv
```

{% hint style="info" %}
Use a `@solana/web3.js` release compatible with VersionedTransaction, TransactionMessage, getLatestBlockhash, and simulateTransaction. APIs may differ across major versions.
{% endhint %}

***

### Configuring environment variables&#x20;

Create a .env file with your GetBlock endpoint:

```bash
# Solana RPC by GetBlock (insert your actual access token)
SOLANA_RPC_URL="https://go.getblock.io/<ACCESS_TOKEN>"

# Jupiter endpoints (use the official dev.jup.ag host or new hostnames)
JUP_QUOTE_API="https://quote-api.jup.ag/v1/quote"
JUP_SWAP_API="https://swap-api.jup.ag/v1/swap"

# Jupiter API key if required by current Jupiter plan
JUP_API_KEY="your_jupiter_api_key_if_any"
```

{% hint style="info" %}
Reminder: Never commit .env to source control. Use a secrets manager.
{% endhint %}

***

### High-level flow&#x20;

Jupiter’s Swap API returns a base64-encoded unsigned `VersionedTransaction`. Deserialize it, sign, optionally simulate, and send the raw serialized bytes.

1. Validate request inputs (inputMint, outputMint, amount in user-facing units, allowed slippage).
2. Convert `amount` to token smallest units (lamports for SOL, token decimals for SPL).
3. Call Jupiter **`/quote`** to get routes. [Pick a route](https://dev.jup.ag/docs/swap-api/get-quote) that fits the slippage and cost requirements.[ ](https://dev.jup.ag/docs/swap-api/get-quote?utm_source=chatgpt.com)
4. Call Jupiter **`/swap`** (POST) with the chosen quote & `userPublicKey` to receive an unsigned, base64 `swapTransaction`.
5. Deserialize to `VersionedTransaction` and optionally fetch Address Lookup Table (ALT) accounts if you need to inspect or modify the message.[ ](https://dev.jup.ag/docs/swap-api/send-swap-transaction?utm_source=chatgpt.com)
6. Run [`simulateTransaction`](../../../api-reference/solana-sol/simulatetransaction-solana.md) to detect likely failures.
7. If the simulation looks good, sign with your KMS/custodial key or the user's signature, then send via your GetBlock Solana RPC API and confirm.&#x20;

***

### Example implementation

This is a runnable sketch for a simple backend function that performs the flow described above. Adapt it to your environment and key-management approach.

```javascript
// src/swap.js
import dotenv from "dotenv";
import axios from "axios";
import {
 Connection,
 Keypair,
 VersionedTransaction,
 sendAndConfirmRawTransaction,
 clusterApiUrl,
 LAMPORTS_PER_SOL,
 TransactionMessage,
 VersionedMessage,
 AddressLookupTableAccount,
} from "@solana/web3.js";

dotenv.config();

// Connect to GetBlock’s Solana RPC API
const connection = new Connection(process.env.SOLANA_RPC_URL, "confirmed");

// Helper: convert SOL amount to lamports
const solToLamports = (sol) => Math.floor(sol * LAMPORTS_PER_SOL);

// Helper: call Jupiter quote API
async function getJupiterQuote({ inputMint, outputMint, amount, slippageBps = 50 }) {
 const q = new URLSearchParams({
   inputMint,
   outputMint,
   amount: amount.toString(),
   slippageBps: slippageBps.toString(),
 });
 const headers = {};
 if (process.env.JUP_API_KEY) headers["Authorization"] = `Bearer ${process.env.JUP_API_KEY}`;
 const url = `${process.env.JUP_QUOTE_API}?${q.toString()}`;
 const res = await axios.get(url, { headers });
 return res.data; // inspect routes
}

// Helper: call Jupiter swap to get swapTransaction (unsigned, base64)
async function buildJupiterSwap({ quoteResponse, userPublicKey, wrapAndUnwrapSol = true }) {
 const headers = { "Content-Type": "application/json" };
 if (process.env.JUP_API_KEY) headers["Authorization"] = `Bearer ${process.env.JUP_API_KEY}`;
 const body = {
   quoteResponse,
   userPublicKey,
   wrapAndUnwrapSol,
 };
 const res = await axios.post(process.env.JUP_SWAP_API, body, { headers });
 return res.data;
}

// Main flow: perform swap (backend-custodial signing)
export async function performSwap({ payerKeypair, inputMint, outputMint, amountSol, slippageBps = 50 }) {
 // 1) Get quote (amount must be in smallest units)
 const amountLamports = solToLamports(amountSol); // if input is SOL
 const quote = await getJupiterQuote({
   inputMint: "So11111111111111111111111111111111111111112", // SOL well-known mint for example
   outputMint,
   amount: amountLamports,
   slippageBps,
 });

 if (!quote || !quote.data || quote.data.length === 0) {
   throw new Error("No routes from Jupiter quote");
 }
 // Choose the best route (example: first)
 const route = quote.data[0];

 // 2) Build swap transaction on Jupiter
 const swapResp = await buildJupiterSwap({
   quoteResponse: route, // Jupiter docs require the original quote response object (or the entire quote)
   userPublicKey: payerKeypair.publicKey.toBase58(),
   wrapAndUnwrapSol: true,
 });

 const { swapTransaction } = swapResp; // base64 unsigned versioned transaction
 if (!swapTransaction) throw new Error("Missing swapTransaction from Jupiter");

 // 3) Deserialize versioned txn
 const txBuf = Buffer.from(swapTransaction, "base64");
 const vtx = VersionedTransaction.deserialize(txBuf);

 // Optional: If the transaction references Address Lookup Tables, you'll need to fetch and attach them
 if (vtx.message.addressTableLookups?.length) {
   const lookupAccounts = await Promise.all(
     vtx.message.addressTableLookups.map(async (lookup) => {
       const acct = await connection.getAccountInfo(lookup.accountKey);
       if (!acct) throw new Error("Failed to fetch ALT account info");
       return new AddressLookupTableAccount({
         key: lookup.accountKey,
         state: AddressLookupTableAccount.deserialize(acct.data),
       });
     })
   );
   // If you intend to decompile the message and modify it, use TransactionMessage.decompile with these lookupAccounts.
   // For simply signing and sending, you generally don't need to modify the lookup tables.
 }


 // 4) OPTIONAL: simulate before signing (simulate accepts VersionedTransaction)
 // note: simulateTransaction expects a versioned tx (may require a valid blockhash inside)
 const sim = await connection.simulateTransaction(vtx, { sigVerify: false, commitment: "processed" });
 if (sim.value.err) {
   console.warn("Simulation error:", sim.value.err);
   // decide whether to proceed, throw, or attempt to get a new quote
   throw new Error("Simulation failed");
 }

 // 5) Sign transaction with required signers (custodial example)
 // NOTE: many Jupiter swaps only require the user's (payer) signature. If swap requires other signatures, include them.
 vtx.sign([payerKeypair]);

 // 6) Send signed raw transaction
 const raw = vtx.serialize();
 const txid = await connection.sendRawTransaction(raw, { skipPreflight: false });
 // Confirm with blockhash+lastValidBlockHeight style 
 const latest = await connection.getLatestBlockhash();
 await connection.confirmTransaction(
   { signature: txid, blockhash: latest.blockhash, lastValidBlockHeight: latest.lastValidBlockHeight },
   "confirmed"
 );

 return txid;
}

```

**Notes**:

* `VersionedTransaction.deserialize(...)` is the recommended path per [Jupiter docs](https://dev.jup.ag/docs/swap-api/send-swap-transaction) (swap response is a base64 versioned transaction).
* Use `connection.simulateTransaction(versionedTx)` to check likely failures and compute-unit usage before spending gas. [`simulateTransaction`](https://docs.getblock.io/api-reference/solana-sol/simulatetransaction-solana) supports versioned transactions.
* For signing: call `vtx.sign([Keypair])`. After signing, send with `sendRawTransaction(vtx.serialize())`. Do not attempt to mutate `tx.recentBlockhash` manually — Jupiter’s returned transaction usually already has a valid blockhash; if you rebuild or modify the message, use [`getLatestBlockhash()`](https://docs.getblock.io/api-reference/solana-sol/sol_getlatestblockhash) to recompile with an up-to-date blockhash.
* See GetBlock and Jupiter docs for rate limits and API-key behavior.

***

### Security considerations

* Do not accept raw private keys from callers.
* Avoid sending a user’s secret key in a request body.
* Rotate API tokens if needed and protect GetBlock access tokens in secrets managers.

***

This guide explains how to build a Node.js backend service that integrates Jupiter’s swap APIs with GetBlock’s Solana RPC to enable programmatic token swaps, aimed at developers building custodial or non-custodial swap endpoints. It focuses on practical implementation details, common pitfalls, and production considerations. Validate the design on a local validator/devnet, before moving to mainnet.
