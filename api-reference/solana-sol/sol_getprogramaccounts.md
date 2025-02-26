---
description: >-
  Example code for the getProgramAccounts json-rpc method. Сomplete guide on how
  to use getProgramAccounts json-rpc in GetBlock.io Web3 documentation.
---

# getProgramAccounts - Solana

{% hint style="success" %}
The getProgramAccounts RPC method allows querying all accounts owned by a specific program on the Solana blockchain.
{% endhint %}

This method is efficient for retrieving program-owned accounts with optional filters, encoding, and slicing configurations. By leveraging the Solana Core API, developers can fetch program data tailored to their Web3 application's requirements.

### **Supported Networks**

The getProgramAccounts method supports the following networks:

* Mainnet
* Devnet

### Parameters

* Pubkey: string (Required)\
  The public key of the program to query, represented as a base-58 encoded string.
* Config: object (Optional)\
  A configuration object containing optional fields:
  * commitment: Specifies the desired state of the Solana network (e.g., "processed", "confirmed", or "finalized").
  * encoding: Specifies the encoding for account data. Supported values are:
    * "base58": For data under 129 bytes (slow).
    * "base64": For data of any size.
    * "base64+zstd": Compresses data using Zstandard and base64-encodes the result.
    * "jsonParsed": Returns human-readable account state data. If no parser is found, it defaults to "base64".
  * dataSlice: An object to limit returned account data, containing:
    * offset: Integer specifying the starting byte position.
    * length: Integer specifying the number of bytes to include.
  * filters: An array of filter objects. Results include only accounts meeting all specified filter criteria. Common filters include:
    * memcmp: Matches account data at a specific offset.
    * dataSize: Matches accounts with a specified data size.\


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
    "method": "getProgramAccounts",
    "params": [
        "4Nd1mBQtrMJVYVfKf2PJy9NZUZdTAsp7D4xWLs4gDB4T",
        null
    ]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns an array of program-owned accounts, including metadata such as lamports, data, and ownership details.

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "pubkey": "8YZt5MXLKh7yo4DQF3VqCSigpFktSoH44o5arVvHzWzC",
            "account": {
                "data": [
                    "AAAAAAEAAAACtzNsyJrW0g==",
                    "base64"
                ],
                "executable": false,
                "lamports": 2039280,
                "owner": "11111111111111111111111111111111",
                "rentEpoch": 215
            }
        }
    ]
}
```

**Response Parameters**

* pubkey: The public key of the account.
* account: An object with the following properties:
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

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    id: 1, 
    method: "getProgramAccounts",
    params: [
        "4Nd1mBQtrMJVYVfKf2PJy9NZUZdTAsp7D4xWLs4gDB4T",
        {
            encoding: "base64",
            filters: [
                {
                    memcmp: {
                        offset: 0,
                        bytes: "12345"
                    }
                },
                {
                    dataSize: 128
                }
            ]
        }
    ]
};

const fetchProgramAccounts = async () => {
    try {
        const response = await axios.post(url, payload, { headers });

        if (response.status === 200) {
            const programAccounts = response.data.result || "No data available";
            console.log("Program Accounts:", programAccounts);
        } else {
            console.error("Unexpected response:", response.status, response.statusText);
        }
    } catch (error) {
        console.error("getProgramAccounts error:", error.response?.data || error.message);
    }
};

fetchProgramAccounts();
```
{% endtab %}
{% endtabs %}

**Error Handling**

Errors during getProgramAccounts requests may occur due to invalid public keys, unsupported configurations, or network issues. Handle errors effectively to ensure reliable Web3 getProgramAccounts application performance. For example, log detailed error messages and retry requests with valid parameters if a getProgramAccounts error arises.

### Integration with Web3

The getProgramAccounts method simplifies program-owned account retrieval for Web3 applications. By leveraging this Solana JSON RPC method, developers can efficiently fetch program data, reducing latency and improving API performance. Use getProgramAccounts RPC Solana to enhance your blockchain explorers, analytics platforms, and decentralized applications.
