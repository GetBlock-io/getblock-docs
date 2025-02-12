---
description: >-
  Example code for the getMultipleAccounts JSON-RPC method. This complete guide
  will help you understand how to use getMultipleAccounts in GetBlock.io Web3
  documentation.
---

# getMultipleAccounts - Solana

{% hint style="success" %}
The getMultipleAccounts RPC Solana method enables querying multiple accounts on the Solana blockchain in a single request.
{% endhint %}

&#x20;This method is efficient for retrieving account data, saving network bandwidth, and ensuring faster performance for Web3 applications. By utilizing the Solana Core API, developers can retrieve data from multiple accounts with customized configurations for encoding and data slicing.

### **Supported Networks**

The getMultipleAccounts method supports the following networks:

* Mainnet
* Devnet

### Parameters

* Pubkeys: array (Required)\
  An array of public keys to query, represented as base-58 encoded strings.
* Config: object (Optional)\
  A configuration object containing optional fields:
  * commitment (optional): Specifies the desired state of the Solana network (e.g., "processed", "confirmed", or "finalized").
  * encoding: Specifies the encoding for account data. Supported values are:
    * "base58": For data under 129 bytes (slow).
    * "base64": For data of any size.
    * "base64+zstd": Compresses data using Zstandard and base64-encodes the result.
    * "jsonParsed": Returns human-readable account state data. If no parser is found, it defaults to "base64".
  * dataSlice: An object to limit returned account data, containing:
    * offset: Integer specifying the starting byte position.
    * length: Integer specifying the number of bytes to include.

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
    "method": "getMultipleAccounts",
    "params": [
        ["vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg", "4fYNw3dojWmQ4dXtSGE9epjRGy9pFSx62YypT7avPYvA"], 
        {
            "dataSlice": {
                "offset": 0,
                "length": 10
            },
            "encoding": "base64"
        }
    ]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns account data for each queried public key, along with metadata such as lamports and ownership details.

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "context": {
            "slot": 1
        },
        "value": [
            {
                "data": [
                    "AAAAAAEAAAACtzNsyJrW0g==",
                    "base64"
                ],
                "executable": false,
                "lamports": 1000000000,
                "owner": "11111111111111111111111111111111",
                "rentEpoch": 2
            },
            {
                "data": [
                    "",
                    "base64"
                ],
                "executable": false,
                "lamports": 5000000000,
                "owner": "11111111111111111111111111111111",
                "rentEpoch": 2
            }
        ]
    }
}
```

**Response Parameters**

* context.slot: The slot number when the response data was retrieved.
* value: An array of account objects with the following properties:
  * data: Account data encoded based on the request configuration.
  * executable: Indicates whether the account is executable.
  * lamports: The amount of lamports held in the account.
  * owner: The program ID responsible for the account.
  * rentEpoch: The epoch during which the account will next be charged rent.

### Use Case

The getMultipleAccounts method in Solana allows developers to retrieve data for multiple accounts in a single RPC request. This makes it an excellent choice for use cases where an application needs to efficiently retrieve state data for several accounts at once, reducing the number of individual requests, saving network bandwidth, and improving performance. Below is a practical use case for this method:

### Code Example

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/api-key"; 
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    id: 1, 
    method: "getMultipleAccounts",
    params: [
        ["vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg", "4fYNw3dojWmQ4dXtSGE9epjRGy9pFSx62YypT7avPYvA"],
        {
            encoding: "base64",
            dataSlice: { offset: 0, length: 10 }
        }
    ]
};

const fetchMultipleAccounts = async () => {
    try {
        const response = await axios.post(url, payload, { headers });

        if (response.status === 200) {
            const accounts = response.data.result?.value || "No data available";
            console.log("Accounts:", accounts);
        } else {
            console.error("Unexpected response:", response.status, response.statusText);
        }
    } catch (error) {
        console.error("getMultipleAccounts error:", error.response?.data || error.message);
    }
};

fetchMultipleAccounts();

```
{% endtab %}
{% endtabs %}

**Error Handling**

Errors during getMultipleAccounts requests may occur due to invalid public keys, unsupported configurations, or network issues. Handle errors effectively to ensure reliable Web3 getMultipleAccounts application performance. For example, log detailed error messages and retry requests with valid parameters if a getMultipleAccounts error arises.

### Integration with Web3

The getMultipleAccounts method simplifies account data retrieval for Web3 applications. By leveraging this Solana JSON RPC method, developers can efficiently fetch account states, reducing latency and improving API performance. Use getMultipleAccounts RPC Solana to enhance your blockchain explorers, analytics platforms, and decentralized applications.
