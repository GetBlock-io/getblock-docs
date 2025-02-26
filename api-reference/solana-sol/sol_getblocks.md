---
description: >-
  The getBlocks method in Solana enables developers to retrieve a range of
  blocks by specifying a starting slot and optionally an ending slot.
---

# getBlocks - Solana

{% hint style="success" %}
The getBlocks method in Solana allows developers to retrieve a range of blocks, specified by the starting and optionally ending slot numbers.
{% endhint %}

&#x20;This method is vital for querying multiple consecutive blocks, making it essential for tasks like batch processing, block data analysis, and historical block retrieval. The getBlocks method is part of Solana's Core API and provides efficient access to block data, enabling applications to track blockchain progress or to verify a series of blocks for various purposes.

### **Supported Networks**

The getBlocks RPC Solana method supports the following network types:

* Mainnet
* Devnet

### Parameters

* start\_slot (u64, required):\
  The first slot to query, represented as a 64-bit unsigned integer. This is the starting point for retrieving block data.
* end\_slot (u64, optional):\
  The last slot to query, represented as a 64-bit unsigned integer. If not provided, the method will retrieve blocks up to the latest available slot.
* commitment (object, optional):\
  Defines the level of finality for the query. The valid options are:
  * "finalized" (default): Ensures that only finalized blocks are returned.
  * "processed" is not supported.

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
    "method": "getBlocks",
    "params": [122791100, 122791185, null]
}'
```
{% endtab %}
{% endtabs %}

### Response

The response contains a list of blocks within the specified range of slots.

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": [
        122791100,
        122791101,
        122791102,
        122791103,
        122791105,
        122791106,
        122791107,
        122791108,
        122791113,
        122791114,
        122791184,
        122791185
    ]
}
```

**Response Parameters**

* id:\
  A unique identifier for the request, matching the ID sent in the request body.
* jsonrpc:\
  Specifies the use of JSON-RPC version 2.0.
* result:\
  An array of block slot numbers within the specified range.

### Use Case

The getBlocks method is helpful in various scenarios, such as:

* Historical Block Retrieval: Retrieve a series of blocks from a specific starting point for analysis.
* Batch Processing: Efficiently fetch multiple blocks for large-scale data processing or validation.
* Blockchain Auditing: Confirm the presence of a series of blocks in specific slot ranges.

### Error Handling

Errors with the getBlocks method may occur under the following conditions:

* Invalid or non-existent slot numbers provided.
* Missing or incorrect API key.
* Unsupported commitment parameter.

#### Example Error Response

```json
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32007,
        "message": "Slot range not found"
    },
    "id": "getblock.io"
}
```

### Code getBlocks Example - Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    id: 1, 
    method: "getBlocks",
    params: [122791100, 122791185, null]
};

const fetchBlocks = async () => {
    try {
        const response = await axios.post(url, payload, { headers });

        if (response.status === 200) {
            console.log("Block Slots:", response.data.result || "No data available");
        } else {
            console.error("Unexpected status:", response.status, response.statusText);
        }
    } catch (error) {
        console.error("Error:", error.response?.data || error.message);
    }
};

fetchBlocks();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

The Web3 getBlocks method is an excellent tool for Web3 developers who need to retrieve a range of blocks quickly and efficiently. By querying a specified slot range, developers can track multiple blocks at once, which is essential for building applications that require historical block data, batch block processing, or blockchain analysis.
