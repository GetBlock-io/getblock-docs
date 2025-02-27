---
description: >-
  The sendTransaction JSON-RPC method broadcasts a signed transaction to the
  Solana network, enabling Web3 sendTransaction operations like token transfers,
  smart contract interactions, and NFT minting
---

# sendTransaction - Solana

{% hint style="success" %}
The sendTransaction RPC Solana method submits a fully signed transaction (encoded as a string) to the blockchain.&#x20;
{% endhint %}

As a core component of Solana’s JSON-RPC framework, it allows developers to execute on-chain actions while controlling parameters like preflight checks, retries, and encoding formats.

Unlike Ethereum’s viem sendTransaction, which handles gas estimation and signing, Solana requires transactions to be pre-signed and serialized. This API method is critical for applications requiring real-time block inclusion, such as exchanges or gaming platforms.

### Supported Networks

Access this method via Solana API Endpoints:

* Mainnet
* Devnet

### Parameters

#### transaction (string, required):

The signed transaction, encoded as a base58 or base64 string.

#### config (object, optional):

Customize transaction handling with these fields:

* skipPreflight (bool, optional): Bypass preflight checks (default: false). Use cautiously to speed up requests.
* preflightCommitment (string, optional): Commitment level for preflight verification (default: finalized).
* encoding (string, optional): Transaction encoding: base58 (deprecated) or base64 (recommended).
* maxRetries (usize, optional): Maximum retry attempts by the RPC node before abandoning the transaction.

### Request

API Endpoints:

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

#### sendTransaction example (cURL)&#x20;

{% tabs %}
{% tab title="curl" %}
```json
curl --location "https://go.getblock.io/<ACCESS-TOKEN>/" -XPOST \
--header "Content-Type: application/json" \
--data '{
   "jsonrpc": "2.0",
   "id": 1,
   "method": "sendTransaction",
   "params": [
       "4hXTCkRzt9WyecNzV1XPgCDfGAZzQKNxLXgynz5QDuWWPSAZBZSHptvWRL3BjCvzUXRdKvHL2b7yGrRQcWyaqsaBCncVG7BFggS8w9snUts67BSh3EqKpXLUm5UMHfD7ZBe9GhARjbNQMLJ1QD3Spr6oMTBU6EhdB4RD8CP2xUxr2u3d6fos36PD98XS6oX8TQjLpsMwncs5DAMiD4nNnR8NBfyghGCWvCVifVwvA8B8TJxE1aiyiv2L429BCWfyzAme5sZW8rDb14NeCQHhZbtNqfXhcp2tAnaAT",
       {"encoding": "base64", "skipPreflight": true}
   ]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the transaction signature. If the blockhash is expired or the node is out of sync, a sendTransaction error occurs.

#### Example Success Response:

```json
{
   "jsonrpc": "2.0",
   "result": "2nBhEBYYvfaAe16UMNqRHre4YNSskvuYgx3M6E4JP1oDYvZEJHvoPzyUidNgNX5r9sTyN1J9UxtbCXy2rqYcuyuv",
   "id": "getblock.io"
}
```

**Example Error Response** (sendTransaction error):

```json
{
   "jsonrpc": "2.0",
   "error": {
       "code": -32005,
       "data": {"numSlotsBehind": 90384},
       "message": "Node is behind by 90384 slots"
   },
   "id": "getblock.io"
}
```

### Error Handling

Common sendTransaction error scenarios include:

* Expired blockhash (transaction too old).
* Insufficient balance for fees.
* Invalid transaction encoding or signature.
* RPC node synchronization issues.

Retry with maxRetries or a fresh blockhash to resolve transient errors.

### Use Cases

The sendTransaction RPC Solana method is ideal for:

* Wallets executing user-initiated transfers or swaps.
* dApps submitting on-chain votes or NFT actions.
* DeFi protocols processing liquidations or staking.
* Games minting in-game assets.

By configuring skipPreflight, developers reduce latency for time-sensitive transactions while accepting higher failure risks.

### Code Example (JavaScript) – Web3 sendTransaction Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };


const payload = {
   jsonrpc: "2.0",
   id: 1, 
   method: "sendTransaction",
   params: [
       "4hXTCkRzt9WyecNzV1XPgCDfGAZzQKNxLXgynz5QDuWWPSAZBZSHptvWRL3BjCvzUXRdKvHL2b7yGrRQcWyaqsaBCncVG7BFggS8w9snUts67BSh3EqKpXLUm5UMHfD7ZBe9GhARjbNQMLJ1QD3Spr6oMTBU6EhdB4RD8CP2xUxr2u3d6fos36PD98XS6oX8TQjLpsMwncs5DAMiD4nNnR8NBfyghGCWvCVifVwvA8B8TJxE1aiyiv2L429BCWfyzAme5sZW8rDb14NeCQHhZbtNqfXhcp2tAnaAT",
       { encoding: "base64", maxRetries: 5 }
   ]
};

const sendTransaction = async () => {
   try {
       const response = await axios.post(url, payload, { headers });

       if (response.status === 200 && response.data.result) {
           console.log("Transaction Signature:", response.data.result);
       } else {
           console.error("sendTransaction error:", response.data.error || "Unknown error");
       }
   } catch (error) {
       console.error("Error:", error.response?.data || error.message);
   }
};

sendTransaction();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

Integrate the sendTransaction RPC Solana method into Web3 applications to enable seamless on-chain interactions. By leveraging Core API parameters like preflightCommitment and maxRetries, developers balance speed and reliability for transactions, ensuring optimal user experiences in wallets, dApps, and trading platforms.

\
