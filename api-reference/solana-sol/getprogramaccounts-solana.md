---
description: >-
  The getProgramAccounts JSON-RPC method retrieves all accounts owned by the
  specified program Pubkey in the Solana blockchain.
---

# getProgramAccounts – Solana

{% hint style="success" %}
The **getProgramAccounts** RPC Solana method allows developers to query multiple accounts belonging to a specific program Pubkey.
{% endhint %}

The getProgramAccounts method retrieves **all accounts associated with a specific program** on the Solana blockchain. It allows developers to efficiently query and filter on-chain data, making it essential for interacting with smart contracts, indexing program-related accounts, and analyzing decentralized application (dApp) states.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet
* Devnet

### Parameters

#### Required Parameters

* **`string`** (required): The Pubkey of the program, provided as a base-58 encoded string.

#### Optional Parameters

* **`object`** (optional): A configuration object containing:
  * **commitment** (`string`, optional): Defines the level of finality for the query.
  * **minContextSlot** (`number`, optional): The minimum slot at which the request can be evaluated.
  * **withContext** (`bool`, optional): Wraps the result in an RpcResponse JSON object.
  * **encoding** (`string`, optional): Specifies the encoding format for the returned account data.
    * Default: `json`
    * Supported values: `jsonParsed`, `base58`, `base64`, `base64+zstd`.
  * **dataSlice** (`object`, optional): Specifies a portion of account data to return.
    * `length` (`usize`): Number of bytes to return.
    * `offset` (`usize`): Byte offset from which to start reading.
  * **filters** (`array`, optional): Allows filtering results using up to 4 filter objects. The resultant accounts must meet all filter criteria.

### Result

By default, the response contains an array of JSON objects. If the `withContext` flag is set, the array is wrapped in an `RpcResponse` JSON object.

Each result object contains:

* `pubkey` (`string`): The account Pubkey, base-58 encoded.
* `account` (`object`):
  * `lamports` (`u64`): Number of lamports assigned to this account.
  * `owner` (`string`): Base-58 encoded Pubkey of the program that owns this account.
  * `data` (`[string, encoding]|object`): Account data, either as encoded binary data or JSON format.
  * `executable` (`bool`): Indicates if the account contains a program.
  * `rentEpoch` (`u64`): The epoch at which this account will next owe rent.
  * `space` (`u64`): The data size of the account.

### Request Example

#### API Endpoints

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

#### cURL Example

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
      {
        "filters": [
          {
            "dataSize": 17
          },
          {
            "memcmp": {
              "offset": 4,
              "bytes": "3Mc6vR"
            }
          }
        ]
      }
    ]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns an array of **accounts owned by the specified program Pubkey**.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "account": {
        "data": "2R9jLfiAQ9bgdcw6h8s44439",
        "executable": false,
        "lamports": 15298080,
        "owner": "4Nd1mBQtrMJVYVfKf2PJy9NZUZdTAsp7D4xWLs4gDB4T",
        "rentEpoch": 28,
        "space": 42
      },
      "pubkey": "CxELquR1gPP8wHe33gZ4QxqGB3sZ9RSwsJ2KshVewkFY"
    }
  ],
  "id": 1
}
```

### Error Handling

Common getProgramAccounts error scenarios:

* Invalid Pubkey: If the provided key is incorrectly formatted.
* Network errors: Connectivity issues with the Solana JSON-RPC API endpoints.
* Malformed request: Incorrectly structured JSON requests.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid Pubkey format"
  },
  "id": 1
}
```

### Use Cases

The Solana **getProgramAccounts** method is useful for:

* **dApp developers**: Fetching all accounts managed by a specific program;
* **Blockchain explorers**: Displaying program-related account data efficiently;
* **Validators and node operators**: Monitoring program accounts in real time.

### Code getProgramAccounts Example – Web3 Integration

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
      filters: [
        { dataSize: 17 },
        {
          memcmp: {
            offset: 4,
            bytes: "3Mc6vR"
          }
        }
      ],
      encoding: "base64"
    }
  ]
};

const fetchProgramAccounts = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && Array.isArray(response.data.result)) {
      const accounts = response.data.result;
      if (accounts.length > 0) {
        accounts.forEach((account, index) => {
          console.log(`Account ${index + 1}:`);
          console.log(`  Pubkey: ${account.pubkey}`);
          console.log(`  Lamports: ${account.account.lamports}`);
          console.log(`  Owner: ${account.account.owner}`);
          console.log(`  Executable: ${account.account.executable}`);
          console.log(`  Data (base64): ${account.account.data}`);
          console.log(`  Rent Epoch: ${account.account.rentEpoch}`);
        });
      } else {
        console.log("No program accounts found for the specified filters.");
      }
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getProgramAccounts error:", error.response?.data || error.message);
  }
};

fetchProgramAccounts();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 **getProgramAccounts** into Solana’s Core API, developers can efficiently track program-owned accounts, retrieve transaction and block details, and optimize decentralized applications. The JSON-RPC request structure allows streamlined account queries, making it an essential tool for Solana-based services.

\
