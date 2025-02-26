---
description: >-
  The getMultipleAccounts JSON-RPC method retrieves account information for a
  list of public keys (Pubkeys) in the Solana blockchain.
---

# getMultipleAccounts – Solana

{% hint style="success" %}
The getMultipleAccounts RPC Solana method provides details about multiple accounts specified by their Pubkeys.
{% endhint %}

This method is particularly useful for dApps, blockchain explorers, and financial analytics tools that need to track multiple accounts efficiently.

The request supports additional parameters such as encoding format, data slicing, and commitment level to refine the results. This method integrates seamlessly with Solana’s Core API, enabling efficient batch retrieval of account states.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet
* Devnet

### Parameters

#### Required Parameters

* array (required): An array of up to 100 Pubkeys, provided as base-58 encoded strings.

#### Optional Parameters

* object (optional): A configuration object containing:
  * commitment (string, optional): Defines the level of finality for the query.
  * minContextSlot (number, optional): The minimum slot at which the request can be evaluated.
  * dataSlice (object, optional): Specifies a portion of account data to return.
    * length (usize): Number of bytes to return.
    * offset (usize): Byte offset from which to start reading.
  * encoding (string, optional): Specifies the encoding format for the returned account data.
    * Default: base64
    * Supported values: jsonParsed, base58, base64, base64+zstd.

### Result

The response is a JSON object with value equal to an array of:

* null: If the account at the provided Pubkey does not exist.
* object: A JSON object containing:
  * lamports (u64): Number of lamports assigned to this account.
  * owner (string): Base-58 encoded Pubkey of the program assigned to the account.
  * data (\[string, encoding]|object): Account data, either as encoded binary data or JSON format.
  * executable (bool): Indicates if the account contains a program.
  * rentEpoch (u64): The epoch at which this account will next owe rent.
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
curl --location "https://go.getblock.io/<ACCESS-TOKEN>/" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getMultipleAccounts",
    "params": [
      [
        "vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg",
        "4fYNw3dojWmQ4dXtSGE9epjRGy9pFSx62YypT7avPYvA"
      ],
      {
        "encoding": "base58"
      }
    ]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns account information for the queried Pubkeys.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": { "apiVersion": "2.0.15", "slot": 341197247 },
    "value": [
      {
        "data": ["", "base58"],
        "executable": false,
        "lamports": 88849814690250,
        "owner": "11111111111111111111111111111111",
        "rentEpoch": 18446744073709551615,
        "space": 0
      },
      {
        "data": ["", "base58"],
        "executable": false,
        "lamports": 998763433,
        "owner": "2WRuhE4GJFoE23DYzp2ij6ZnuQ8p9mJeU6gDgfsjR4or",
        "rentEpoch": 18446744073709551615,
        "space": 0
      }
    ]
  },
  "id": 1
}
```

### Error Handling

Common getMultipleAccounts error scenarios:

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

The Solana getMultipleAccounts method is useful for:

* Web3 wallets: Fetching balances and ownership details for multiple accounts.
* Blockchain explorers: Displaying batch account data efficiently.
* Validators and node operators: Monitoring multiple accounts in real time.

### Code getMultipleAccounts Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getMultipleAccounts",
  params: [
    [
      "vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg",
      "4fYNw3dojWmQ4dXtSGE9epjRGy9pFSx62YypT7avPYvA"
    ],
    { "encoding": "base58" }
  ]
};

const fetchMultipleAccounts = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result?.value) {
      const accounts = response.data.result.value;
      accounts.forEach((account, index) => {
        if (account) {
          console.log(`Account ${index + 1}:`);
          console.log(`  Data: ${account.data}`);
          console.log(`  Lamports: ${account.lamports}`);
          console.log(`  Owner: ${account.owner}`);
          console.log(`  Executable: ${account.executable}`);
          console.log(`  Rent Epoch: ${account.rentEpoch}`);
        } else {
          console.log(`Account ${index + 1}: Not found`);
        }
      });
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getMultipleAccounts error:", error.response?.data || error.message);
  }
};

fetchMultipleAccounts();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 getMultipleAccounts into Solana’s Core API, developers can retrieve multiple transaction and block details in a single request, optimizing performance for dApps, analytics platforms, and infrastructure services. The JSON-RPC request allows efficient bulk account queries, making it an essential tool for Solana applications.

\
