---
description: >-
  The getTokenAccountsByDelegate JSON-RPC method retrieves all SPL Token
  accounts associated with a specified delegate.
---

# getTokenAccountsByDelegate – Solana

{% hint style="success" %}
The **getTokenAccountsByDelegate** RPC Solana method allows developers to query SPL Token accounts where the provided account Pubkey has been designated as a delegate.
{% endhint %}

The method supports filtering by mint or programId and can return data in various encoding formats, making it a flexible tool for **account tracking and analysis** in Solana’s Core API.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet

### Parameters

#### Required Parameters

* **`string`** (required): The Pubkey of the delegate account to query, as a base-58 encoded string.

#### Optional Parameters

* **`object`** (optional): A JSON object containing one of the following fields:
  * `mint` (`string`): The Pubkey of a specific token Mint to filter accounts.
  * `programId` (`string`): The Pubkey of the Token Program that owns the accounts.
* **`object`** (optional): A configuration object containing:
  * **commitment** (`string`): The level of commitment for the request.
  * **minContextSlot** (`number`): The minimum slot at which the request can be evaluated.
  * **dataSlice** (`object`): Requests a slice of the account’s data.
    * `length` (`usize`): Number of bytes to return.
    * `offset` (`usize`): Byte offset from which to start reading.
  * **encoding** (`string`): The encoding format for the account data.
    * Supported values: `base58`, `base64`, `base64+zstd`, `jsonParsed`.

### Result

The response is an RpcResponse JSON object containing:

* **`context`** (`object`): Provides contextual information about the slot.
  * `slot` (`u64`): The slot number when the data was retrieved.
* **`value`** (`array`): An array of JSON objects representing the token accounts.
  * Each object includes:
    * `pubkey` (`string`): The account Pubkey in base-58 encoding.
    * `account` (`object`):
      * `lamports` (`u64`): Number of lamports in the account.
      * `owner` (`string`): The Pubkey of the program owning the account.
      * `data` (`object`): Encoded account data or parsed JSON representation.
      * `executable` (`bool`): Whether the account contains a program.
      * `rentEpoch` (`u64`): The epoch at which the account will next owe rent.
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
    "method": "getTokenAccountsByDelegate",
    "params": [
      "4Nd1mBQtrMJVYVfKf2PJy9NZUZdTAsp7D4xWLs4gDB4T",
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

A successful request returns an array of SPL Token accounts associated with the specified delegate.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 1114
    },
    "value": [
      {
        "account": {
          "data": {
            "program": "spl-token",
            "parsed": {
              "info": {
                "tokenAmount": {
                  "amount": "1",
                  "decimals": 1,
                  "uiAmount": 0.1,
                  "uiAmountString": "0.1"
                },
                "delegate": "4Nd1mBQtrMJVYVfKf2PJy9NZUZdTAsp7D4xWLs4gDB4T",
                "delegatedAmount": {
                  "amount": "1",
                  "decimals": 1,
                  "uiAmount": 0.1,
                  "uiAmountString": "0.1"
                },
                "state": "initialized",
                "isNative": false,
                "mint": "3wyAj7Rt1TWVPZVteFJPLa26JmLvdb1CAKEFZm3NY75E",
                "owner": "CnPoSPKXu7wJqxe59Fs72tkBeALovhsCxYeFwPCQH9TD"
              },
              "type": "account"
            },
            "space": 165
          },
          "executable": false,
          "lamports": 1726080,
          "owner": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
          "rentEpoch": 4,
          "space": 165
        },
        "pubkey": "28YTZEwqtMHWrhWcvv34se7pjS7wctgqzCPB3gReCFKp"
      }
    ]
  },
  "id": 1
}
```

### Error Handling

Common getTokenAccountsByDelegate error scenarios:

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

The Solana **getTokenAccountsByDelegate** method is useful for:

* **Wallet applications**: Tracking delegated SPL Tokens for user accounts;
* **Web3 analytics tools**: Monitoring token delegations across multiple accounts;
* **DeFi applications**: Auditing token delegation activity;
* **Blockchain explorers**: Displaying delegated accounts in real-time.

### Code getTokenAccountsByDelegate Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getTokenAccountsByDelegate",
  params: [
    "4Nd1mBQtrMJVYVfKf2PJy9NZUZdTAsp7D4xWLs4gDB4T",
    {
      programId: "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"
    },
    { encoding: "jsonParsed" }
  ]
};

const fetchTokenAccountsByDelegate = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && Array.isArray(response.data.result?.value)) {
      const accounts = response.data.result.value;
      if (accounts.length > 0) {
        accounts.forEach((account, index) => {
          console.log(`Account ${index + 1}:`);
          console.log(`  Pubkey: ${account.pubkey}`);
          console.log(`  Token Amount: ${account.account.data.parsed.info.tokenAmount.uiAmount}`);
          console.log(`  Mint: ${account.account.data.parsed.info.mint}`);
          console.log(`  Owner: ${account.account.data.parsed.info.owner}`);
        });
      } else {
        console.log("No token accounts found for the specified delegate.");
      }
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getTokenAccountsByDelegate error:", error.response?.data || error.message);
  }
};

fetchTokenAccountsByDelegate();
```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 **getTokenAccountsByDelegate** into Solana’s Core API, developers can efficiently track delegated token accounts, monitor transaction activity, and manage account state. This JSON-RPC method is essential for applications that handle SPL Token delegations in Web3 environments.
