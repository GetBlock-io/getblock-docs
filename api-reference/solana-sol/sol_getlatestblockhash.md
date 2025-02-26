---
description: >-
  The getLatestBlockhash JSON-RPC method retrieves the most recent blockhash and
  its validity duration on the Solana blockchain.
---

# getLatestBlockhash - Solana

{% hint style="success" %}
The getLatestBlockhash RPC Solana method returns the latest blockhash and its associated lastValidBlockHeight, which indicates the last slot at which the blockhash can be used.
{% endhint %}

&#x20;As part of Solana’s Core API, it is critical for applications requiring real-time transaction submission, such as wallets or DeFi platforms.

This method supports optional parameters like commitment to specify the confirmation level of the blockhash. By ensuring that transactions reference valid blockhashes, developers prevent errors and optimize Web3 application performance.

### Supported Networks

Access this method via Solana API Endpoints:

* Mainnet
* Devnet

### Parameters

* commitment (object, optional): Specifies the confirmation level for retrieving the blockhash. Supported options:
*
  * finalized (default): The blockhash is confirmed and immutable.
  * confirmed: The blockhash is confirmed but may still be reorganized.
  * processed: Not supported for this method.

### Request

#### API Endpoint:

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

#### Example (cURL) – getLatestBlockhash example:

{% tabs %}
{% tab title="curl" %}
```json
curl --location "https://go.getblock.io/<ACCESS-TOKEN>/" -XPOST \
--header "Content-Type: application/json" \
--data '{
   "jsonrpc": "2.0",
   "id": 1,
   "method": "getLatestBlockhash",
   "params": [{"commitment": "finalized"}]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the latest blockhash and its validity details.

#### Example Response:

```json
{
   "jsonrpc": "2.0",
   "result": {
       "context": { "slot": 123456789 },
       "value": {
           "blockhash": "J3jJv...",
           "lastValidBlockHeight": 123456800
       }
   },
   "id": "getblock.io"
}
```

#### Key Response Fields:

* blockhash: The latest blockhash (base-58 encoded string).
* lastValidBlockHeight: The last slot at which the blockhash is valid.
* context.slot: The slot at which the blockhash was retrieved.

### Error Handling

Common getLatestBlockhash error scenarios include:

* Invalid commitment level (e.g., processed).
* Missing or incorrect API key.
* Network connectivity issues.

#### Error Response Example:

```json
{
   "jsonrpc": "2.0",
   "error": {
       "code": -32602,
       "message": "Invalid commitment level"
   },
   "id": "getblock.io"
}
```

### Use Case

The getLatestBlockhash RPC Solana method is ideal for:

* Wallets ensuring valid transaction submissions.
* dApps requiring real-time blockhash updates.
* DeFi platforms processing time-sensitive transactions.
* Explorers displaying the latest network state.

By referencing a valid blockhash, developers prevent transaction failures and improve user experiences.

### Code getLatestBlockhash Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');


const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };


const payload = {
   jsonrpc: "2.0",
   id: 1, 
   method: "getLatestBlockhash",
   params: [{ "commitment": "finalized" }]
};


const fetchLatestBlockhash = async () => {
   try {
       const response = await axios.post(url, payload, { headers });

       if (response.status === 200 && response.data.result?.value) {
           const { blockhash, lastValidBlockHeight } = response.data.result.value;
           console.log("Latest Blockhash:", blockhash);
           console.log("Last Valid Block Height:", lastValidBlockHeight);
       } else {
           console.error("Unexpected response:", response.data);
       }
   } catch (error) {
       console.error("getLatestBlockhash error:", error.response?.data || error.message);
   }
};

fetchLatestBlockhash();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

Integrate the getLatestBlockhash RPC Solana method into Web3 applications to ensure valid transaction submissions and real-time network state tracking. By leveraging Core API parameters like commitment, developers optimize blockhash retrieval for wallets, dApps, and enterprise solutions.

\
