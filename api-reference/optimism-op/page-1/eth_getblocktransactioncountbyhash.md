# eth\_getBlockTransactionCountByHash

{% hint style="success" %}
The **eth\_getBlockTransactionCountByHash** method returns the transaction count in a specific block identified by its hash on Optimism.
{% endhint %}

This method retrieves the number of transactions contained in a block using the block's hash. It's useful for quickly determining block activity without fetching the full block data. On Optimism, blocks typically contain fewer transactions than Ethereum L1 due to the faster block time (\~2 seconds).

### Supported Networks

The eth\_getBlockTransactionCountByHash **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
One parameter is required:
{% endhint %}

| Parameter | Type           | Description          | Required |
| --------- | -------------- | -------------------- | -------- |
| blockHash | DATA, 32 Bytes | The hash of a block. | Yes      |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_getBlockTransactionCountByHash **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
{"jsonrpc": "2.0",
"method": "eth_getBlockTransactionCountByHash",
"params": ["0xe8f38578b756a9fd05ca05dcbe56fd57ce72e9fb06a90e5064562baccf7c7a6e"], 
"id": "getblock.io"
}
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_getBlockTransactionCountByHash", "params": ["0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2"], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x26"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The number of transactions in the block as a hexadecimal string. `0x15` equals 21 transactions.

### Use Case

The eth\_getBlockTransactionCountByHash method is commonly used for:

* **Block analysis**
* **Transaction throughput monitoring**
* **Network activity tracking**

### Code Example

Here's how to use the eth\_getBlockTransactionCountByHash method with different programming languages:

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
    "method": "eth_getBlockTransactionCountByHash",
    "params": ["0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getBlockTransactionCountByHash result:", result)
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
    method: "eth_getBlockTransactionCountByHash",
    params: ["0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getBlockTransactionCountByHash result:", response.data.result);
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
