---
description: >-
  Example code for the getBlocksWithLimit json-rpc method. Сomplete guide on how
  to use getBlocksWithLimit json-rpc in GetBlock.io Web3 documentation.
---

# getBlocksWithLimit - Solana

{% hint style="success" %}
The getBlocksWithLimit RPC Solana method in Solana allows developers to retrieve a sequence of block numbers within a specified range.
{% endhint %}

It is a vital JSON-RPC method for accessing blocks efficiently, making it particularly useful for applications that need to fetch transaction data, analyze blockchain activity, or monitor network performance. This Web3 method simplifies interaction with the Solana blockchain by providing a clear structure for block retrieval. The getBlocksWithLimit RPC Solana method supports multiple networks like Mainnet and Devnet.

The getBlocksWithLimit method is an essential feature of Solana's Core API. It allows developers to specify parameters such as the starting slot (start\_slot) and optionally define an ending slot or commitment level to fine-tune the request. By leveraging the flexibility of this method, developers can easily retrieve and analyze transaction data from multiple blocks in one request.

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
curl --location "https://sol.getblock.io/mainnet" -XPOST \
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

### Code Example

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

// Define the API URL and headers
const url = "https://sol.getblock.io/mainnet";
const headers = { 
    "Content-Type": "application/json", 
    "x-api-key": "YOUR-API-KEY" 
};

// Prepare the request payload
const payload = {
    jsonrpc: "2.0",
    method: "getBlocksWithLimit",
    params: [5, 10, null],
    id: "getblock.io"
};

// Send the POST request
axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            // Extract block numbers from the response
            const blockNumbers = response.data.result;
            console.log("Block Numbers:", blockNumbers);
        } else {
            // Handle unexpected HTTP statuses
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

