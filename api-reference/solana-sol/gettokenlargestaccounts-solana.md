---
description: >-
  The getTokenLargestAccounts JSON-RPC method retrieves the 20 largest accounts
  for a specified SPL Token type.
---

# getTokenLargestAccounts – Solana

{% hint style="success" %}
The **getTokenLargestAccounts** RPC Solana method returns a list of the 20 largest token accounts associated with a specific token Mint address.
{% endhint %}

This information helps developers **analyze token distribution patterns** and **track high-value accounts** using the Core API.

The response includes account addresses, raw balances, and formatted balances with decimals applied, which can be displayed in user-friendly interfaces.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet

### Parameters

#### Required Parameters

* **`string`** (required): The Pubkey of the token Mint to query, provided as a base-58 encoded string.

#### Optional Parameters

* **`object`** (optional): A configuration object containing:
  * `commitment` (string): Defines the level of finality for the request.

### Result

The response returns an RpcResponse object containing:

* **`context`** (`object`): Provides contextual information about the slot.
  * **`slot`** (u64): The slot number when the largest token accounts were retrieved.
* **`value`** (`array`): An array of JSON objects representing the largest token accounts.
  * Each object includes:
    * `address` (`string`): The Pubkey of the token account.
    * `amount` (`string`): The raw token balance (without decimals).
    * `decimals` (u8`)`: The number of decimal places for the token balance.
    * `uiAmount` (`number`|`null`): The token balance with decimals applied. (Deprecated)
    * `uiAmountString` (`string`): The token balance as a string with decimals applied.

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
    "method": "getTokenLargestAccounts",
    "params": [
      "3wyAj7Rt1TWVPZVteFJPLa26JmLvdb1CAKEFZm3NY75E"
    ]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns the largest token accounts with their balances.

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
        "address": "FYjHNoFtSQ5uijKrZFyYAxvEr87hsKXkXcxkcmkBAf4r",
        "amount": "771",
        "decimals": 2,
        "uiAmount": 7.71,
        "uiAmountString": "7.71"
      },
      {
        "address": "BnsywxTcaYeNUtzrPxQUvzAWxfzZe3ZLUJ4wMMuLESnu",
        "amount": "229",
        "decimals": 2,
        "uiAmount": 2.29,
        "uiAmountString": "2.29"
      }
    ]
  },
  "id": 1
}
```

In this response:

* `address`: The Pubkey of each token account.
* `amount`: The raw balance in lamports.
* `decimals`: The number of decimal places.
* `uiAmountString`: The formatted balance as a string.

### Error Handling

Common getTokenLargestAccounts error scenarios:

* Invalid Mint Pubkey: If the provided Pubkey is invalid.
* Network issues: Connectivity problems with the Solana JSON-RPC API endpoints.
* Invalid request parameters: Incorrect parameter structure or data types.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid token Mint Pubkey"
  },
  "id": 1
}
```

### Use Cases

The Solana **getTokenLargestAccounts** method is useful for:

* **Token distribution analysis**: Identifying whale accounts holding significant token amounts;
* **DeFi applications**: Tracking liquidity pool balances;
* **Web3 analytics tools**: Displaying token holder rankings;
* **Wallet applications**: Providing users insights into major token holders.

### Code getTokenLargestAccounts Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getTokenLargestAccounts",
  params: [
    "3wyAj7Rt1TWVPZVteFJPLa26JmLvdb1CAKEFZm3NY75E"
  ]
};

const fetchTokenLargestAccounts = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && Array.isArray(response.data.result?.value)) {
      const accounts = response.data.result.value;
      if (accounts.length > 0) {
        accounts.forEach((account, index) => {
          console.log(`Account ${index + 1}:`);
          console.log(`  Address: ${account.address}`);
          console.log(`  Amount: ${account.amount}`);
          console.log(`  Decimals: ${account.decimals}`);
        });
      } else {
        console.log("No largest token accounts found for the specified mint.");
      }
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getTokenLargestAccounts error:", error.response?.data || error.message);
  }
};

fetchTokenLargestAccounts();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 **getTokenLargestAccounts** into Solana’s Core API, developers can efficiently analyze token distributions, monitor account balances, and provide insights into transaction activity. This JSON-RPC method is a key tool for applications dealing with token analytics and blockchain monitoring.
