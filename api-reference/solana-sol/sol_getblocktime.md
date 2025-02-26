---
description: >-
  The getBlockTime method retrieves the estimated production time of a block by
  its slot number, returning a UNIX timestamp.
---

# getBlockTime - Solana

{% hint style="success" %}
The getBlockTime method in Solana retrieves the estimated production time of a specific block, identified by its slot.&#x20;
{% endhint %}

This method is a key feature of Solana’s Core API, enabling developers to associate blocks with their approximate timestamps. It is essential for applications requiring time-based analysis of blockchain events, transaction tracking, or monitoring network activity.

This Web3 method offers a simple and efficient way to retrieve time-related data for blocks on the Solana blockchain by providing a single parameter, the block (slot number).\
The getBlockTime method is a crucial part of the Solana getBlockTime RPC in the Core API. Developers can use this method to obtain the block time associated with a specific block. The getBlockTime error response is typically returned when the block is not available or when there’s a mismatch in the parameters.

### **Supported Networks**

The getAccountInfo RPC Solana method supports the following network types:

* Mainnet
* Devnet

### Parameters

1. block (u64, required):
   * The slot number of the block to query, represented as a 64-bit unsigned integer.

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
    "method": "getBlockTime",
    "params": [122791192]
}'
```
{% endtab %}
{% endtabs %}

### Response

The response contains the estimated UNIX timestamp for the block, returned as a 64-bit integer.

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": 1646011316
}
```

**Response Parameters**

1. id:
   * A unique identifier for the request, matching the ID sent in the request body.
2. jsonrpc:
   * Specifies the use of JSON-RPC version 2.0.
3. result:
   * The block's estimated production time, returned as a UNIX timestamp (seconds since January 1, 1970, UTC).

### Use Case

The getBlockTime method is crucial for a variety of applications, including:

* Transaction analysis: Determining the approximate time of transactions included in a specific block.
* Blockchain monitoring: Analyzing the time intervals between blocks for performance monitoring or optimization.
* Application integration: Adding time-based metadata to transactions or blocks in wallets, explorers, or analytics tools

### Error Handling

Errors with the getBlockTime method may occur under the following conditions:

* The block parameter refers to a non-existent or invalid slot.
* The provided API key is missing, expired, or invalid.

#### Example Error Response:

```json
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32007,
        "message": "Slot 122791192 not found"
    },
    "id": "getblock.io"
}
```

### Code getBlockTime Example

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    id: 1,
    method: "getBlockTime",
    params: [122791192]
};

const fetchBlockTime = async () => {
    try {
        const response = await axios.post(url, payload, { headers });

        if (response.status === 200) {
            const blockTime = response.data.result;

            if (blockTime) {
                console.log("Block Time (UNIX):", blockTime);
                console.log("Block Time (Date):", new Date(blockTime * 1000));
            } else {
                console.log("No block time data available");
            }
        } else {
            console.error("Unexpected status:", response.status, response.statusText);
        }
    } catch (error) {
        console.error("Error:", error.response?.data || error.message);
    }
};

fetchBlockTime();
```
{% endtab %}
{% endtabs %}

### Integration with Web3

The Web3 getBlockTime method is a powerful JSON-RPC tool for accessing time-based data in Solana. By querying the block parameter, developers can map slot numbers to approximate timestamps, making it easier to correlate blockchain activity with real-world events. This method is commonly used in Web3 applications for tasks such as historical data analysis, transaction time tracking, and building time-sensitive features.
