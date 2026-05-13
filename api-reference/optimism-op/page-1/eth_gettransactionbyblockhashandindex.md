# eth\_getTransactionByBlockHashAndIndex

{% hint style="success" %}
The **eth\_getTransactionByBlockHashAndIndex** method returns a transaction from a specific block on Optimism using the block hash and transaction index.
{% endhint %}

This method retrieves a specific transaction from a block by combining the block's hash with the transaction's index position within that block. The index is zero-based, so the first transaction has index 0. This is useful when you need to access transactions in order within a block or iterate through all transactions in a specific block.

## Supported Networks

The eth\_getTransactionByBlockHashAndIndex **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

## Parameters

{% hint style="info" %}
Two parameters are required:
{% endhint %}

| Parameter        | Type           | Description                                    | Required |
| ---------------- | -------------- | ---------------------------------------------- | -------- |
| blockHash        | DATA, 32 Bytes | The hash of a block.                           | Yes      |
| transactionIndex | QUANTITY       | The transaction index position in hexadecimal. | Yes      |

## Request

### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_getTransactionByBlockHashAndIndex **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockHashAndIndex",
    "params": ["0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2", "0x0"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_getTransactionByBlockHashAndIndex", "params": ["0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2", "0x0"], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

## Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "hash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
        "blockHash": "0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2",
        "blockNumber": "0x7A69B2C",
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e",
        "to": "0x8ba1f109551bD432803012645Ac136dff489ca2c",
        "value": "0xde0b6b3a7640000"
    }
}
```

## Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: A transaction object with all transaction details, or null if not found.

## Use Case

The eth\_getTransactionByBlockHashAndIndex method is commonly used for:

* **Sequential transaction processing**
* **Block transaction iteration**
* **Transaction ordering analysis**

## Code Example

Here's how to use the eth\_getTransactionByBlockHashAndIndex method with different programming languages:

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockHashAndIndex",
    "params": ["0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2", "0x0"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getTransactionByBlockHashAndIndex result:", result)
else:
    print("Error:", response.status_code, response.text)
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    method: "eth_getTransactionByBlockHashAndIndex",
    params: ["0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2", "0x0"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getTransactionByBlockHashAndIndex result:", response.data.result);
        } else {
            console.error("Error:", response.status, response.statusText);
        }
    })
    .catch(error => {
        console.error("Error:", error.response ? error.response.data : error.message);
    });
```
{% endtab %}
{% endtabs %}

If an error occurs, it could indicate connection issues, invalid parameters, or network problems. Always implement proper error handling in your applications.
