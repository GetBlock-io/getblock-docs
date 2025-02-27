---
description: >-
  The getBlocksWithLimit method in Solana retrieves a list of confirmed blocks
  within a specified range, starting from a given block slot.
---

# getBlocksWithLimit - Solana

{% hint style="success" %}
The getBlocksWithLimit RPC Solana method in Solana allows developers to retrieve a sequence of block numbers within a specified range.
{% endhint %}

The getBlocksWithLimit method is a key part of Solana’s Core API, enabling efficient block retrieval for transaction analysis, blockchain monitoring, and network performance tracking. It supports multiple networks, including Mainnet and Devnet, allowing developers to specify a starting slot and optionally refine the request with additional parameters. This method simplifies batch block data retrieval, making it a valuable tool for blockchain-based applications.

### **Supported Networks**

The getBlocksWithLimit RPC Solana method supports the following network types:

* Mainnet
* Devnet

### Parameters

1. start\_slot (u64, required):
   * The first slot to query, provided as a 64-bit unsigned integer.
2. end\_slot (u64, optional):
   * The last slot to query, provided as a 64-bit unsigned integer. If omitted, the method retrieves blocks up to the limit.
3. commitment (object, optional):
   * Specifies the desired level of commitment. Options include:
     * "finalized": Ensures the block data is fully confirmed and immutable.
     * "processed" is not supported for this method. If not provided, the default commitment is "finalized".

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
    "method": "getBlocksWithLimit",
    "params": [5, 10, null]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful response provides a list of block numbers within the specified range.

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": [
        122257143,
        122257144,
        122257145,
        122257146,
        122257147,
        122257148,
        122257149,
        122257150,
        122257151,
        122257152
    ]
}
```

**Response Parameters**

1. id: A unique identifier for the request, matching the ID sent in the request body.
2. jsonrpc: Specifies the use of JSON-RPC version 2.0.
3. result: An array of block numbers retrieved between the start\_slot and end\_slot values.

### Error Handling

Errors with the getBlocksWithLimit method can occur if:

* Invalid start\_slot or end\_slot values are provided.
* The commitment level "processed" is specified (this is unsupported).
* The API key is missing or invalid.

getBlocksWithLimit example error Response:

```json
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32602,
        "message": "Invalid parameters"
    },
    "id": "getblock.io"
}
```

### Use Case

The getBlocksWithLimit method is ideal for applications requiring a sequential range of blocks for analysis or processing. For instance:

* Explorers can use it to display transactions from a specific range of blocks.
* Analytical tools can retrieve data to calculate transaction statistics or monitor network activity.
* Wallets can use it to verify the inclusion of transactions in blocks.

By specifying parameters like start\_slot and end\_slot, developers can retrieve only the required block data, optimizing their applications for performance and efficiency.

### Code getBlocksWithLimit Example - Web3 Integration &#x20;

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { 
    "Content-Type": "application/json", 
    "x-api-key": "YOUR-API-KEY" 
};

const payload = {
    jsonrpc: "2.0",
    method: "getBlocksWithLimit",
    params: [5, 10, null],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            const blockNumbers = response.data.result;
            console.log("Block Numbers:", blockNumbers);
        } else {
            console.error("Error:", response.status, response.statusText);
        }
    })
    .catch(error => {
        // Handle network or server errors
        console.error("Error:", error.response ? error.response.data : error.message);
    });
```
{% endtab %}
{% endtabs %}

### Integration with Web3

The Web3 getBlocksWithLimit method is a powerful tool for Web3 getBlocksWithLimit applications interacting with Solana's Core API. By enabling efficient retrieval of block numbers, it supports various use cases such as block exploration, transaction monitoring, and network analysis. With support for parameters like start\_slot, end\_slot, and commitment, developers can tailor requests to meet their specific needs.

Integrating the getBlocksWithLimit JSON-RPC method into applications provides seamless access to Solana block data, making it a cornerstone for blockchain-based solutions.

