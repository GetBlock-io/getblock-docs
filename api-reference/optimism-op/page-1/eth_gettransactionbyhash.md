# eth\_getTransactionByHash

{% hint style="success" %}
The **eth\_getTransactionByHash** method returns detailed information about a transaction identified by its hash on Optimism.
{% endhint %}

This method retrieves comprehensive transaction data including the sender, recipient, value, gas parameters, input data, and block information. It's one of the most commonly used methods for tracking transaction status and retrieving transaction details. The method returns null if the transaction is not found. On Optimism, the transaction object may include additional L2-specific fields.

### Supported Networks

The eth\_getTransactionByHash **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
One parameter is required:
{% endhint %}

| Parameter       | Type           | Description                | Required |
| --------------- | -------------- | -------------------------- | -------- |
| transactionHash | DATA, 32 Bytes | The hash of a transaction. | Yes      |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_getTransactionByHash **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_getTransactionByHash", "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

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
        "value": "0xde0b6b3a7640000",
        "gas": "0x5208",
        "gasPrice": "0x3b9aca00",
        "input": "0x",
        "nonce": "0x1",
        "transactionIndex": "0x0",
        "type": "0x2"
    }
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: A transaction object with details including hash, blockHash, blockNumber, from, to, value, gas, gasPrice, input data, nonce, and type.

### Use Case

The eth\_getTransactionByHash method is commonly used for:

* **Transaction tracking**
* **Payment verification**
* **Debugging transactions**
* **Transaction history display**

### Code Example

Here's how to use the eth\_getTransactionByHash method with different programming languages:

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
    "method": "eth_getTransactionByHash",
    "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getTransactionByHash result:", result)
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
    method: "eth_getTransactionByHash",
    params: ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getTransactionByHash result:", response.data.result);
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
