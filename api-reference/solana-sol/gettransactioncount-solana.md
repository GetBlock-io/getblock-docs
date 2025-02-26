---
description: >-
  The Web3 getTransactionCount JSON-RPC method retrieves the total number of
  transactions confirmed on the Solana network up to the latest block.
---

# getTransactionCount - Solana

{% hint style="success" %}
The Solana getTransactionCount RPC method returns the total count of transactions processed by the network, reflecting its growth and activity. As part of Solana’s Core API, it is critical for developers building network explorers, performance trackers, or applications requiring block-level analytics.
{% endhint %}

The method normally returns confirmed transaction counts, but can be extended to include pending transactions by using a specific call to getTransactionCount pending.

This method supports commitment parameters to determine data finality. Unlike Ethereum’s eth\_getTransactionCount (which may trigger errors like AttributeError: 'Eth' object has no attribute 'gettransactioncount'), Solana’s implementation focuses on network-wide totals rather than per-account transactions.

### Supported Networks

This method is accessible via Solana API endpoints:

* Mainnet
* Devnet

### Parameters

* commitment (object, optional): Specifies the confirmation level. Supported options:
  * finalized (default): Returns the count of transactions in finalized blocks.
  * confirmed: Uses the latest confirmed block.
  * processed: Not supported for this method.

### Request

#### API Endpoint:

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

#### getTransactionCount example Request (cURL):

{% tabs %}
{% tab title="curl" %}
```json
curl --location "https://go.getblock.io/<ACCESS-TOKEN>/" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "method": "getTransactionCount",
    "params": [{"commitment": "finalized"}],
    "id": "getblock.io"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the total transaction count as an integer value.

#### Example Response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": 60547116336
}
```

#### Response Parameters:

* result: Total number of transactions confirmed up to the specified block.

### Error Handling

Common getTransactionCount error scenarios include:

* Using unsupported commitment levels like processed.
* Invalid API key or incorrect endpoints.
* Ethereum-specific errors (e.g., AttributeError: 'Eth' object has no attribute 'gettransactioncount' when using the Solana API).

#### Example Error Response:

```json
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32602,
        "message": "Unsupported commitment: processed"
    },
    "id": "getblock.io"
}
```

### Use Cases

The getTransactionCount RPC method is ideal for:

* Network dashboards displaying real-time transaction throughput.
* Analytics platforms calculating TPS (transactions per second).
* Developers monitoring blockchain health and activity.
* Auditors verifying historical block data completeness.

### Code Example (JavaScript) – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');


const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };


const payload = {
  jsonrpc: "2.0",
  method: "getTransactionCount",
  params: [{ commitment: "finalized" }],
  id: "getblock.io"
};


const fetchTransactionCount = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result !== undefined) {
      const txCount = response.data.result;
      console.log("Total Transactions (Finalized):", txCount);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getTransactionCount error:", error.response?.data || error.message);
  }
};

fetchTransactionCount();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

Integrate the getTransactionCount RPC Solana method into Web3 applications to track network scalability and performance metrics. By combining this method with block or transaction-specific queries, developers gain holistic insights into Solana’s value as a high-throughput blockchain.

\
