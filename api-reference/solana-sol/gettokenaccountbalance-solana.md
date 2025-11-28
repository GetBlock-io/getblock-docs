---
description: >-
  The getTokenAccountBalance JSON-RPC method retrieves the token balance of a
  specified SPL Token account.
---

# getTokenAccountBalance – Solana

{% hint style="info" %}
The **getTokenAccountBalance** RPC Solana method provides the balance of an SPL Token account using a given Pubkey.
{% endhint %}

The response includes the raw balance, the number of decimal places, and formatted balances. This method is particularly useful in **dApps**, **wallets**, and **financial applications** using the Solana Core API.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet

### Parameters

#### Required Parameters

* **`string`** (required): The Pubkey of the Token account to query, provided as a base-58 encoded string.

#### Optional Parameters

* **`object`** (optional): A configuration object containing:
  * **commitment** (`string`, optional): Defines the level of finality for the request.

### Result

The response returns an RpcResponse object containing:

* **`context`** (`object`): Contextual information about the slot.
  * `slot` (`u64`): The slot number when the balance was retrieved.
* **`value`** (`object`):
  * `amount` (`string`): The raw balance without decimals (string representation of u64).
  * `decimals` (`u8`): The number of base 10 digits to the right of the decimal point.
  * `uiAmount` (`number`|`null`): The balance with decimals applied. (Deprecated)
  * `uiAmountString` (`string`): The balance formatted as a string with decimals applied.

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
    "method": "getTokenAccountBalance",
    "params": [
      "7fUAJdStEuGbc3sM84cKRL6yYaaSstyLSU4ve5oovLS7"
    ]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns the token balance of the specified account.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 1114
    },
    "value": {
      "amount": "9864",
      "decimals": 2,
      "uiAmount": 98.64,
      "uiAmountString": "98.64"
    }
  },
  "id": 1
}
```

In this response:

* The raw balance is 9864.
* With 2 decimal places, the formatted balance is 98.64.

### Error Handling

Common getTokenAccountBalance error scenarios:

* Invalid Pubkey: If the provided Pubkey is incorrectly formatted.
* Network errors: Connectivity issues with the Solana JSON-RPC API endpoints.
* Non-existent token account: If the provided Pubkey does not correspond to a valid SPL Token account.

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

The Solana getTokenAccountBalance method is useful for:

* **Wallet applications**: Displaying users' token balances;
* **Web3 analytics tools**: Monitoring token balances across accounts;
* **DeFi applications**: Calculating token holdings for smart contracts;
* **Blockchain explorers**: Displaying account balances.

### Code getTokenAccountBalance Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');


const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getTokenAccountBalance",
  params: [
    "7fUAJdStEuGbc3sM84cKRL6yYaaSstyLSU4ve5oovLS7"
  ]
};

const fetchTokenAccountBalance = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result?.value) {
      const balance = response.data.result.value;
      console.log("Token Account Balance:");
      console.log(`  Amount: ${balance.amount}`);
      console.log(`  Decimals: ${balance.decimals}`);
      console.log(`  UI Amount: ${balance.uiAmount}`);
      console.log(`  UI Amount String: ${balance.uiAmountString}`);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getTokenAccountBalance error:", error.response?.data || error.message);
  }
};

fetchTokenAccountBalance();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 **getTokenAccountBalance** into Solana’s Core API, developers can easily query token balances, track account activity, and analyze transaction flows. This JSON-RPC method is essential for applications requiring accurate and up-to-date token balance information.
