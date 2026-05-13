# eth\_getBlockTransactionCountByNumber

{% hint style="success" %}
The **eth\_getBlockTransactionCountByNumber** method returns the transaction count in a specific block identified by its number on Optimism.
{% endhint %}

This method retrieves the number of transactions contained in a block using the block number or tags like 'latest', 'earliest', or 'pending'. It provides a quick way to assess block activity without downloading the entire block.

### Supported Networks

The eth\_getBlockTransactionCountByNumber **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
One parameter is required:
{% endhint %}

| Parameter   | Type          | Description                                              | Required |
| ----------- | ------------- | -------------------------------------------------------- | -------- |
| blockNumber | QUANTITY\|TAG | Block number in hex, or 'latest', 'earliest', 'pending'. | Yes      |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_getBlockTransactionCountByNumber **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockTransactionCountByNumber",
    "params": ["latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_getBlockTransactionCountByNumber", "params": ["latest"], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x8"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The number of transactions in the block as a hexadecimal string. `0x8` equals 8 transactions.

### Use Case

The eth\_getBlockTransactionCountByNumber method is commonly used for:

* **Block analysis**
* **Real-time activity monitoring**
* **Network throughput assessment**

### Code Example

Here's how to use the eth\_getBlockTransactionCountByNumber method with different programming languages:

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
    "method": "eth_getBlockTransactionCountByNumber",
    "params": ["latest"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getBlockTransactionCountByNumber result:", result)
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
    method: "eth_getBlockTransactionCountByNumber",
    params: ["latest"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getBlockTransactionCountByNumber result:", response.data.result);
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
