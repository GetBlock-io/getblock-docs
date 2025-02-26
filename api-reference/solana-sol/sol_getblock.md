---
description: >-
  The getBlock method retrieves detailed information about a specific block on
  the Solana blockchain, including its height, timestamp, hash, rewards, and
  transactions.
---

# getBlock - Solana

{% hint style="success" %}
The getBlock method allows developers to query a specific block in the Solana blockchain by providing the block’s slot number.
{% endhint %}

It returns detailed information about the block, including its height, timestamp, hash, parent slot, previous block hash, rewards, and transactions contained within the block. This method is particularly useful for obtaining a snapshot of a specific block, helping developers analyze and verify blockchain activity.

### **Supported Networks**

The getBlock RPC Solana method supports the following networks:

* Mainnet
* Devnet

### Parameters

* Slot (u64, required): The slot number of the block you want to query. It should be passed as a 64-bit unsigned integer.
* Config (optional, object): Configuration object that provides additional options for the block query:
  * encoding: The encoding for account data. Can be one of the following:
    * "base58": Slow, suitable for small Account data.
    * "base64": Standard encoding for Account data of any size.
    * "base64+zstd": Compresses Account data using Zstandard and base64-encodes it.
    * "jsonParsed": Attempts to parse Account data into a more human-readable format.
  * transaction.message.instructions: If jsonParsed is requested but no parser is available, this field will fall back to regular JSON encoding for accounts, data, and programIdIndex fields.
  * transactionDetails: Level of transaction detail to return. Options are:
    * "full": Returns complete transaction details (default).
    * "signatures": Returns only the signatures.
    * "none": Does not include transaction details.
  * field will fall back to regular JSON encoding for accounts, data, and programIdIndex fields.
  * transactionDetails: Level of transaction detail to return. Options are:
  * rewards: A boolean indicating whether to include rewards data in the response. Defaults to true.
  * commitment: Defines the level of commitment for the block query, with the default set to "finalized".

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
    "method": "getBlock",
    "params": [122788843, null]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful response contains detailed information about the block, including the block height, block time, blockhash, parent slot, previous blockhash, rewards, and transactions.

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHeight": 111018262,
        "blockTime": 1646009934,
        "blockhash": "Bmm7S3gKAbYLKqEWjFjoZ7PnDGB3C6vsJ1yjG6tznrCX",
        "parentSlot": 122788842,
        "previousBlockhash": "3XhDMjAQMcMJHgbb6aTG48MLCTHnDfwk7FHpJ4wN4ymj",
        "rewards": [
            {
                "commission": null,
                "lamports": -11967,
                "postBalance": 1464864,
                "pubkey": "52GwEMWZ5oTBF6qCNuE1Y5SNDheSjSyjARiFNEUbrsEL",
                "rewardType": "Rent"
            },
            {
                "commission": null,
                "lamports": 6722500,
                "postBalance": 4694146146,
                "pubkey": "DAHJgPKdmncYW8DmY6meaU953a7SktQ7eDGtWduC8W8m",
                "rewardType": "Fee"
            }
        ],
        "transactions": [
            {
                "meta": {
                    "err": null,
                    "fee": 5000,
                    "innerInstructions": [],
                    "logMessages": [
                        "Program FsJ3A3u2vn5cTVofAjvy6y5kwABJAqYWpe4975bi2epH invoke [1]",
                        "Program FsJ3A3u2vn5cTVofAjvy6y5kwABJAqYWpe4975bi2epH consumed 32275 of 200000 compute units",
                        "Program FsJ3A3u2vn5cTVofAjvy6y5kwABJAqYWpe4975bi2epH success"
                    ],
                    "status": {
                        "Ok": null
                    }
                },
                "transaction": {
                    "message": {
                        "accountKeys": [
                            "5YXnWX6Mmd8hp7fCpAB3wQUrHt6WtjJrA5QjmBuySsDP"
                        ],
                        "instructions": [
                            {
                                "accounts": [0, 1, 2],
                                "data": "6mJFQAEssWECZ33ewGbTZAvtVaeV9QBGxMZvAabzAeqe7ffgxn9zbR",
                                "programIdIndex": 3
                            }
                        ]
                    },
                    "signatures": [
                        "2YBeDREvfocgiwmtwE7j2yNWj8vbSG3Uxw17HKYp2f2iNoBy3ps7MuTdQ31PPY5AmAEghgoKJbTGUn25m3SUY96c"
                    ]
                }
            }
        ]
    }
}

```

**Response Parameters**

* id: A unique request identifier, matching the ID sent in the request body.
* jsonrpc: Specifies the use of JSON-RPC version 2.0.
* result:
  * blockHeight: The block’s height.
  * blockTime: The block’s timestamp.
  * blockhash: The block’s hash.
  * parentSlot: The parent slot number.
  * previousBlockhash: The previous block’s hash.
  * rewards: An array of reward details for the block, including the lamports earned and the account that received them.
  * transactions: An array of transactions included in the block, each with metadata and transaction details.

### Use Case

The getBlock method is highly beneficial for developers working on blockchain analytics, block explorers, or any Solana-based decentralized applications (dApps) that require detailed information about specific blocks. It allows the retrieval of transaction data, reward information, and other block-related details in real time, which is essential for building rich user experiences.

### Code getBlock Example

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    id: 1, 
    method: "getBlock",
    params: [
        122788843,
        null
    ]
};

const fetchBlockInfo = async () => {
    try {
        const response = await axios.post(url, payload, { headers });

        if (response.status === 200) {
            const blockInfo = response.data.result;
            console.log("Block Height:", blockInfo?.blockHeight || "No data available");
            console.log("Block Time:", blockInfo?.blockTime || "No data available");
        } else {
            console.error("Unexpected status:", response.status, response.statusText);
        }
    } catch (error) {
        console.error("Error:", error.response?.data || error.message);
    }
};

fetchBlockInfo();

```
{% endtab %}
{% endtabs %}

This code example demonstrates how to make a request to the getBlock method, fetch block data, and handle the response. It prints the block's height and time as part of the output.

### Integration with Web3

The Web3 getBlock method is an essential tool for retrieving block information from the Solana blockchain. By querying a block by its slot, developers can gain detailed insights into blockchain activity, including transaction data and rewards. The method’s flexibility in configuring encoding and transaction details makes it an important tool for building sophisticated Web3 applications and blockchain analytics solutions.
