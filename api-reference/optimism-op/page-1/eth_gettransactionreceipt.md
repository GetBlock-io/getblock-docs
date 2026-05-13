# eth\_getTransactionReceipt

{% hint style="success" %}
The **eth\_getTransactionReceipt** method returns the receipt of a mined transaction on Optimism, including status, logs, and L1 fee information.
{% endhint %}

This method retrieves the transaction receipt which contains information about the transaction's execution including status (success/failure), gas used, logs emitted, and contract address if it was a contract creation. Receipts are only available for mined transactions. On Optimism, the receipt includes additional fields for L1 gas information: l1GasUsed, l1GasPrice, l1Fee, and l1FeeScalar, which are essential for understanding the full transaction cost.

## Supported Networks

The eth\_getTransactionReceipt **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

## Parameters

{% hint style="info" %}
One parameter is required:
{% endhint %}

| Parameter       | Type           | Description                | Required |
| --------------- | -------------- | -------------------------- | -------- |
| transactionHash | DATA, 32 Bytes | The hash of a transaction. | Yes      |

## Request

### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_getTransactionReceipt **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionReceipt",
    "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_getTransactionReceipt", "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"], "id": "getblock.io"}
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
        "transactionHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
        "blockHash": "0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2",
        "blockNumber": "0x7A69B2C",
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e",
        "to": "0x8ba1f109551bD432803012645Ac136dff489ca2c",
        "gasUsed": "0x5208",
        "cumulativeGasUsed": "0x5208",
        "status": "0x1",
        "logs": [],
        "logsBloom": "0x0000...",
        "l1GasUsed": "0x5e0",
        "l1GasPrice": "0x3b9aca00",
        "l1Fee": "0x16a3f5b9c0",
        "l1FeeScalar": "1.0"
    }
}
```

## Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: A receipt object containing transactionHash, blockHash, blockNumber, from, to, gasUsed, status (0x1 for success, 0x0 for failure), logs array, and Optimism-specific L1 fee fields.

## Use Case

The eth\_getTransactionReceipt method is commonly used for:

* **Transaction confirmation**
* **Event log processing**
* **Contract deployment verification**
* **Gas usage analysis**
* **L1 fee calculation**

## Code Example

Here's how to use the eth\_getTransactionReceipt method with different programming languages:

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
    "method": "eth_getTransactionReceipt",
    "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getTransactionReceipt result:", result)
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
    method: "eth_getTransactionReceipt",
    params: ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getTransactionReceipt result:", response.data.result);
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
