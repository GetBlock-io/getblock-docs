---
description: >-
  Example code for the getTransactionCount json-rpc method. Сomplete guide on
  how to use getTransactionCount json-rpc in GetBlock.io Web3 documentation.
---

# getTransactionCount - Solana

{% hint style="success" %}
The getTransactionCount JSON-RPC method allows you to retrieve the total number of transactions that have been processed on the Solana blockchain.
{% endhint %}

This method is useful for applications that need to track transaction volume, ensure consistent data indexing, or provide real-time metrics on the Solana network.

By calling this method, developers can easily determine how many transactions have been recorded on the blockchain, which is essential for analytics tools, explorers, and Web3 applications that require detailed network metrics.

### **Supported Networks**

The getTransactionCount method supports the following networks:

* Mainnet
* Devnet

### Parameters

commitment (Optional)

* Type: Object
* Description: This parameter specifies the commitment level for the request. The commitment level defines how up-to-date the information should be. Common values are finalized, confirmed, and processed.
  * finalized: Ensures the data is finalized.
  * confirmed: Ensures that the block and transaction are confirmed by the network.
  * processed: Returns data from any block, whether confirmed or not.

### Request

URL(Endpoints)

<pre class="language-json" data-full-width="false"><code class="lang-json"><strong>https://go.getblock.io/&#x3C;ACCESS-TOKEN>/
</strong></code></pre>

### Example (cURL):

{% tabs %}
{% tab title="curl" %}
```json
curl --location "https://go.getblock.io/<ACCESS-TOKEN>/" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getTransactionCount",
    "params": [null]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful response will return the total number of transactions that have been processed on the Solana blockchain.

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": 60547116336
}
```

**Response Parameters**

* id: A unique request identifier, matching the ID sent in the request body.
* jsonrpc: Specifies the use of JSON-RPC version 2.0.
* result: The total transaction count on the Solana blockchain.

### Use Case

The getTransactionCount method is crucial for applications that need to monitor the volume of transactions occurring on the Solana blockchain. Possible use cases include:

* Blockchain Explorers: Tools that show real-time statistics about the blockchain, such as the total transaction count.
* Analytics Platforms: Services that track blockchain performance and trends over time.
* Web3 Applications: Decentralized applications that need to fetch transaction data for reporting or analytics purposes.

### Code Example

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    id: 1, 
    method: "getTransactionCount",
    params: [null]
};

const fetchTransactionCount = async () => {
    try {
        const response = await axios.post(url, payload, { headers });

        if (response.status === 200) {
            const transactionCount = response.data.result || "No data available";
            console.log("Transaction Count:", transactionCount);
        } else {
            console.error("Unexpected response:", response.status, response.statusText);
        }
    } catch (error) {
        console.error("getTransactionCount error:", error.response?.data || error.message);
    }
};

fetchTransactionCount();

```
{% endtab %}
{% endtabs %}

#### Error Handling

When using the getTransactionCount method, there may be errors related to network issues, invalid API keys, or incorrect endpoints. To handle these situations, developers should implement error-handling logic, such as retrying the request on failure or returning fallback data if the request fails.

#### Integration with Web3

The getTransactionCount method is a key part of Solana's Core API and plays a vital role in providing real-time transaction data to Web3 applications. By utilizing this method, Web3 applications can ensure they have access to accurate transaction statistics for real-time analysis, blockchain performance monitoring, and application functionality.
