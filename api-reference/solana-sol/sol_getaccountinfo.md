---
description: >-
  Example code for the getAccountInfo json-rpc method. Сomplete guide on how to
  use getAccountInfo json-rpc in GetBlock.io Web3 documentation.
---

# getAccountInfo - Solana

{% hint style="success" %}
The getAccountInfo method in Solana retrieves all details associated with a given account. It offers a comprehensive view of the account's data, owner, lamports, and more.
{% endhint %}

This method is particularly useful for applications that need to fetch account-specific information efficiently, such as wallets, explorers, or analytical tools. With flexible encoding options, developers can tailor the returned data to suit their needs. The getAccountInfo RPC Solana method is crucial for understanding account parameters and is a central part of Solana's Core API.

### **Supported Networks**

The getAccountInfo RPC Solana method supports the following network types:

* Mainnet
* Devnet

### Parameters

* Pubkey (string, required): The base-58 encoded public key of the account to query.
* Config (object, optional):
  * Commitment (optional): The level of commitment desired (e.g., "processed", "confirmed", or "finalized").
  * encoding (string, optional): The encoding format for the account data. Options include:
    * "base58": Base-58 encoding for data less than 129 bytes (slower).
    * "base64": Base-64 encoding for larger data.
    * "base64+zstd": Base-64 encoding with Zstandard compression.
    * "jsonParsed": Returns more human-readable account state data (fallback to "base64" if a parser is unavailable).
  * dataSlice (object, optional): Limits the returned account data using:
    * offset (number): Starting byte offset.
    * length (number): Number of bytes to return.

### Request

URL(Endpoints)

<pre class="language-json" data-full-width="false"><code class="lang-json"><strong>https://go.getblock.io/&#x3C;ACCESS-TOKEN>/
</strong></code></pre>

### Example (cURL):

{% tabs %}
{% tab title="curl" %}
```json
curl --location "<ENDPOINT>" -XPOST \
--header 'Content-Type: application/json' \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getAccountInfo",
    "params": [
        "vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg",
        {
            "encoding": "base58"
        }
    ]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful response provides detailed information about the queried account. The response includes getAccountInfo error handling and can help in debugging issues with account data retrieval.

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x"
}
```

**Response Parameters**

* id: A unique request identifier, matching the ID sent in the request body.
* jsonrpc: Specifies the use of JSON-RPC version 2.0.
* result:
  * context:
    * slot: The current slot number.
  * value:
    * data: Account data encoded as specified in the request (e.g., base58).
    * executable: Boolean indicating if the account contains a program.
    * lamports: Number of lamports owned by the account.
    * owner: Base-58 encoded public key of the program that owns the account.
    * rentEpoch: Epoch during which the account will next owe rent.

### Use Case

The getAccountInfo method is ideal for applications needing detailed information about a specific Solana account. For instance, wallets can use it to display account balances, explorers can provide insights into account activity, and developers can analyze program state data. By specifying the encoding type, applications can balance readability and data size for their specific use cases. This makes the getAccountInfo method a key feature in the development of Web3 getAccountInfo applications.getAccountInfo example on javascript

### Code Example

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/c8de489a140b4b80a0ed13f264cc4f65/mainnet";
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    id: 1,  
    method: "getAccountInfo",
    params: [
        "vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg",
        {
            encoding: "base58"
        }
    ]
};

const fetchAccountInfo = async () => {
    try {
        const response = await axios.post(url, payload, { headers });

        if (response.status === 200) {
            console.log("Account Information:", response.data.result);
        } else {
            console.error("Unexpected status:", response.status, response.statusText);
        }
    } catch (error) {
        console.error("Error:", error.response?.data || error.message);
    }
};

fetchAccountInfo();

```
{% endtab %}
{% endtabs %}

This example demonstrates how to query an account's information programmatically using the getAccountInfo method, offering insights into its current state and associated data. Developers can customize encoding and other optional parameters to optimize their requests.

### Integration with Web3

The getAccountInfo method is an essential tool for working with the Solana getAccountInfo Core API, processing JSON-RPC requests, and analyzing account parameters. With support for different Method Endpoints, developers can seamlessly integrate this method into Web3 getAccountInfo applications, enabling them to retrieve transaction and block data for Solana accounts efficiently. By tailoring the parameters and value returned by the request, developers can optimize their applications for various use cases, from wallets to explorers and beyond.



