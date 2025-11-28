---
description: >-
  The getTransaction method in Solana retrieves detailed information about a
  specific transaction using its signature. The response includes metadata,
  status, block confirmations, and other details.
---

# getTransaction - Solana

{% hint style="success" %}
The **getTransaction** method in JSON-RPC is used to fetch details of a confirmed transaction in the Solana blockchain by its signature.
{% endhint %}

The **getTransaction** method is a crucial part of Solana’s Core API, allowing developers to **retrieve detailed information about a specific transaction**. It provides essential data, including transaction status, block confirmations, signatures, and executed instructions.

This method is particularly useful for transaction tracking, debugging, and blockchain activity analysis, making it a key tool for explorers, wallets, and other blockchain-based applications.

#### **Supported Networks** <a href="#supported-networks" id="supported-networks"></a>

The **getTransaction** RPC Solana method supports the following network types:

* Mainnet

#### Parameters <a href="#parameters" id="parameters"></a>

1. **`signature`: `string` (required)**

The transaction signature encoded in base-58.

2. **`config`: `object` (optional)**

A configuration object with additional parameters:

* **`encoding`**: Specifies the format of the returned data. Possible values:
  * "`json`": Default value. Returns transaction data in JSON format.
  * "`jsonParsed`": Parses instructions into a more readable format. Defaults to "`json`" if a parser is unavailable.
  * "`base58`": Encodes raw transaction data in base-58. Slower than other formats.
  * "`base64`": Encodes raw transaction data in base-64.
* **`commitment`**: Specifies the desired state of the Solana network.
  * Possible values: "`finalized`" (default). Returns only finalized transactions.

#### Request <a href="#request" id="request"></a>

URL(Endpoints)

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

#### Example (cURL): <a href="#example-curl" id="example-curl"></a>

{% tabs %}
{% tab title="curl" %}
```json
curl --location "https://go.getblock.io/<ACCESS-TOKEN>/" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getTransaction",
    "params": [
        "2nBhEBYYvfaAe16UMNqRHre4YNSskvuYgx3M6E4JP1oDYvZEJHvoPzyUidNgNX5r9sTyN1J9UxtbCXy2rqYcuyuv",
        "json"
    ]
}'
```
{% endtab %}
{% endtabs %}

#### Response <a href="#response" id="response"></a>

Successful Response:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "slot": 123456,
        "transaction": {
            "message": {
                "accountKeys": [
                    "4Nd1mBQtrMJVYVfKf2PJy9NZUZdTAsp7D4xWLs4gDB4T",
                    "11111111111111111111111111111111"
                ],
                "instructions": [
                    {
                        "programId": "11111111111111111111111111111111",
                        "accounts": [
                            "4Nd1mBQtrMJVYVfKf2PJy9NZUZdTAsp7D4xWLs4gDB4T"
                        ],
                        "data": "AQIDBA=="
                    }
                ],
                "recentBlockhash": "5EWsWhtN1yoUmE1cc6fJxF3k1Zf2Jtq2xNmRpZy1H8FA"
            },
            "signatures": [
                "2nBhEBYYvfaAe16UMNqRHre4YNSskvuYgx3M6E4JP1oDYvZEJHvoPzyUidNgNX5r9sTyN1J9UxtbCXy2rqYcuyuv"
            ]
        },
        "meta": {
            "err": null,
            "fee": 5000,
            "preBalances": [
                1000000000,
                500000000
            ],
            "postBalances": [
                999995000,
                500000000
            ]
        }
    }
}
```

**Response Parameters**

**transaction**

* `signatures`: An array of transaction signatures.
* `message`: An object containing:
  * `accountKeys`: Public keys involved in the transaction.
  * `instructions`: Instructions executed by programs in the transaction.
  * `recentBlockhash`: The block hash used for transaction confirmation.

**meta**

* `err`: Indicates whether an error occurred during the transaction (`null` if successful).
* `fee`: Transaction fee in lamports.
* `preBalances`: Account balances before the transaction.
* `postBalances`: Account balances after the transaction.

#### Use Cases <a href="#use-case" id="use-case"></a>

retrievesThe **getTransaction** method is used to retrieve detailed information about a confirmed transaction on the Solana blockchain by its signature. This method allows developers to integrate transaction lookup functionality into applications such as **blockchain explorers**, **analytics tools**, and **decentralized apps**.

For example, a user inputs a transaction signature, and the application sends a request to the server to fetch data about account balances before and after the transaction, instructions executed, fees, and any potential errors. This enables the display of complete transaction details, enhancing user interaction.

#### Code getTransaction Example - Web3 Integration <a href="#code-gettransaction-example-web3-integration" id="code-gettransaction-example-web3-integration"></a>

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    id: 1,
    method: "getTransaction",
    params: [
        "2nBhEBYYvfaAe16UMNqRHre4YNSskvuYgx3M6E4JP1oDYvZEJHvoPzyUidNgNX5r9sTyN1J9UxtbCXy2rqYcuyuv",
        "json"
    ]
};

const fetchTransaction = async () => {
    try {
        const response = await axios.post(url, payload, { headers });

        if (response.status === 200) {
            const transactionDetails = response.data.result || "No data available";
            console.log("Transaction Details:", transactionDetails);
        } else {
            console.error("Unexpected response:", response.status, response.statusText);
        }
    } catch (error) {
        console.error("getTransaction error:", error.response?.data || error.message);
    }
};

fetchTransaction();
```
{% endtab %}
{% endtabs %}

#### Error Handling <a href="#error-handling" id="error-handling"></a>

When using the getTransaction method, errors may occur due to incorrect signatures, unsupported configurations, or network issues. For reliable application performance, consider:

* Logging detailed error messages.
* Retrying requests with corrected parameters in case of errors.
* Validating the transaction signature and encoding parameters before sending the request.

#### Integration <a href="#integration" id="integration"></a>

The getTransaction method enables developers to retrieve essential transaction details, which can be utilized in blockchain explorers, analytical tools, and decentralized applications. Use this Solana JSON-RPC method for accurate and reliable data retrieval.
