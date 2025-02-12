---
description: >-
  The getBlockHeight method retrieves the current block height of the Solana
  blockchain, representing the latest confirmed block.
---

# getBlockHeight - Solana

{% hint style="success" %}
The getBlockHeight RPC method allows you to fetch the current block height on the Solana blockchain.&#x20;
{% endhint %}

By using this method, you can easily get the latest block number, which represents the most recent block added to the blockchain. The block height is helpful for tracking blockchain progress, synchronizing nodes, or monitoring the state of the network.

### **Supported Networks**

The getBlockHeight RPC method is available on the following networks:

* Mainnet
* Devnet

### Parameters

* Commitment (optional, object): An optional parameter to specify the level of commitment (e.g., "confirmed", "finalized"). If not provided, it defaults to the latest block.

### Request

URL(Endpoints)

<pre class="language-json" data-full-width="false"><code class="lang-json"><strong>https://go.getblock.io/&#x3C;ACCESS-TOKEN>/
</strong></code></pre>

### Example (cURL):

{% tabs %}
{% tab title="curl" %}
```json
curl --location "https://go.getblock.io/api-key" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getBlockHeight",
    "params": [null]
}'
```
{% endtab %}
{% endtabs %}

### Response

The response will return the current block height as a number, which corresponds to the most recent confirmed block in the Solana blockchain.

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": 111246105
}
```

**Response Parameters**

* id: A unique request identifier that matches the ID sent in the request body.
* jsonrpc: Specifies the use of JSON-RPC version 2.0.
* result: The current block height (the most recent confirmed block), represented as an integer

### Use Case

The getBlockHeight method is useful for applications that need to monitor or display the latest state of the blockchain. For instance, it can be used in:

* Block explorers to display the most recent block.
* Wallets or staking platforms to show the latest blockchain status.
* Node operators for synchronization or tracking the progress of the network.

### Code Example

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://sol.getblock.io/YOUR-API-KEY/mainnet";
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    id: 1, 
    method: "getBlockHeight",
    params: [null]
};

const fetchBlockHeight = async () => {
    try {
        const response = await axios.post(url, payload, { headers });

        if (response.status === 200) {
            const blockHeight = response.data.result;
            console.log("Current Block Height:", blockHeight || "No data available");
        } else {
            console.error("Unexpected status:", response.status, response.statusText);
        }
    } catch (error) {
        console.error("Error:", error.response?.data || error.message);
    }
};

fetchBlockHeight();

```
{% endtab %}
{% endtabs %}

This example demonstrates how to query the current block height using the getBlockHeight method, which provides valuable information about the latest block on the Solana blockchain.

### Integration with Web3

The getBlockHeight method is a core part of Solana's Web3 functionality. It allows developers to track the current block height in real-time. This method is essential for applications such as block explorers, wallets, and any Solana-based project that needs to monitor blockchain progress or synchronize with the network. By using this method, developers can stay up to date with the latest block data.
