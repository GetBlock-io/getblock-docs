---
description: >-
  The getTokenAccountsByOwner JSON-RPC method retrieves all SPL Token accounts
  associated with a specified token owner.
---

# getTokenAccountsByOwner – Solana

{% hint style="success" %}
The getTokenAccountsByOwner RPC Solana method allows developers to query SPL Token accounts by specifying an account Pubkey of the owner.&#x20;
{% endhint %}

This method supports optional filtering by mint or programId and offers various encoding formats for flexibility when working with token data in Solana’s Core API.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet
* Devnet

### Parameters

#### Required Parameters

* string (required): The Pubkey of the token owner, provided as a base-58 encoded string.
* object (required): A JSON object containing one of the following fields:
  * mint (string): The Pubkey of a specific token Mint to filter accounts.
  * programId (string): The Pubkey of the Token Program that owns the accounts.

#### Optional Parameters

* object (optional): A configuration object containing:
  * commitment (string): The level of commitment for the request.
  * minContextSlot (number): The minimum slot at which the request can be evaluated.
  * dataSlice (object): Requests a slice of the account’s data.
    * length (usize): Number of bytes to return.
    * offset (usize): Byte offset from which to start reading.
  * encoding (string): The encoding format for the account data.
    * Supported values: base58, base64, base64+zstd, jsonParsed.

### Result

The response is an RpcResponse JSON object containing:

* context (object): Provides contextual information about the slot.
  * apiVersion (string): The version of the API.
  * slot (u64): The slot number when the data was retrieved.
* value (array): An array of JSON objects representing the token accounts.
  * Each object includes:
    * pubkey (string): The account Pubkey in base-58 encoding.
    * account (object):
      * lamports (u64): Number of lamports in the account.
      * owner (string): The Pubkey of the program owning the account.
      * data (object): Encoded account data or parsed JSON representation.
      * executable (bool): Whether the account contains a program.
      * rentEpoch (u64): The epoch at which the account will next owe rent.
      * space (u64): The data size of the account.

### Request Example

#### API Endpoints

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

#### cURL Example

{% tabs %}
{% tab title="curl" %}
```json
curl --location "https://go.getblock.io/api-key" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getTokenAccountsByOwner",
    "params": [
      "A1TMhSGzQxMr1TboBKtgixKz1sS6REASMxPo1qsyTSJd",
      {
        "programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"
      },
      {
        "encoding": "jsonParsed"
      }
    ]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns an array of SPL Token accounts associated with the specified owner.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": { "apiVersion": "2.0.15", "slot": 341197933 },
    "value": [
      {
        "account": {
          "data": {
            "parsed": {
              "info": {
                "isNative": false,
                "mint": "2cHr7QS3xfuSV8wdxo3ztuF4xbiarF6Nrgx3qpx3HzXR",
                "owner": "A1TMhSGzQxMr1TboBKtgixKz1sS6REASMxPo1qsyTSJd",
                "state": "initialized",
                "tokenAmount": {
                  "amount": "420000000000000",
                  "decimals": 6,
                  "uiAmount": 420000000.0,
                  "uiAmountString": "420000000"
                }
              },
              "type": "account"
            },
            "program": "spl-token",
            "space": 165
          },
          "executable": false,
          "lamports": 2039280,
          "owner": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
          "rentEpoch": 18446744073709551615,
          "space": 165
        },
        "pubkey": "BGocb4GEpbTFm8UFV2VsDSaBXHELPfAXrvd4vtt8QWrA"
      }
    ]
  },
  "id": 1
}
```

### Error Handling

Common getTokenAccountsByOwner error scenarios:

* Invalid Pubkey: If the provided Pubkey is incorrectly formatted.
* Network errors: Connectivity issues with the Solana JSON-RPC API endpoints.
* Invalid encoding: If an unsupported encoding type is specified.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid Token Account Pubkey"
  },
  "id": 1
}
```

### Use Cases

The Solana getTokenAccountsByOwner method is useful for:

* Wallet applications: Displaying token balances for user accounts.
* Web3 analytics tools: Tracking token balances across multiple accounts.
* DeFi applications: Analyzing token holdings and transfers.
* Blockchain explorers: Displaying token account information in real-time.

### Code getTokenAccountsByOwner Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/api-key";
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getTokenAccountsByOwner",
  params: [
    "A1TMhSGzQxMr1TboBKtgixKz1sS6REASMxPo1qsyTSJd",
    {
      programId: "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"
    },
    { encoding: "jsonParsed" }
  ]
};

const fetchTokenAccountsByOwner = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && Array.isArray(response.data.result?.value)) {
      const accounts = response.data.result.value;
      if (accounts.length > 0) {
        accounts.forEach((account, index) => {
          console.log(`Account ${index + 1}:`);
          console.log(`  Pubkey: ${account.pubkey}`);
          console.log(`  Mint: ${account.account.data.parsed.info.mint}`);
          console.log(`  Owner: ${account.account.data.parsed.info.owner}`);
          console.log(`  Token Amount: ${account.account.data.parsed.info.tokenAmount.uiAmount}`);
          console.log(`  Decimals: ${account.account.data.parsed.info.tokenAmount.decimals}`);
        });
      } else {
        console.log("No token accounts found for the specified owner.");
      }
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getTokenAccountsByOwner error:", error.response?.data || error.message);
  }
};

fetchTokenAccountsByOwner();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 getTokenAccountsByOwner into Solana’s Core API, developers can efficiently track token accounts, monitor transaction activity, and manage account state. This JSON-RPC method is essential for applications that handle SPL Token balances and ownership records in Web3 environments.
