# trace\_replayBlockTransactions

{% hint style="success" %}
The **trace\_replayBlockTransactions** method replays all transactions in a block with specified trace types on Optimism.
{% endhint %}

This method replays all transactions in a block and returns the requested trace information. You can specify which types of traces you want: `'vmTrace'` for VM execution steps, `'trace'` for call hierarchy, and `'stateDiff'` for state changes.

### Supported Networks

The trace\_replayBlockTransactions **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
Two parameters are required:
{% endhint %}

| Parameter   | Type          | Description                                                  | Required |
| ----------- | ------------- | ------------------------------------------------------------ | -------- |
| blockNumber | QUANTITY\|TAG | Block number in hex, or `'latest'`, `'earliest'`.            | Yes      |
| traceTypes  | Array         | Array of trace types: `'vmTrace'`, `'trace'`, `'stateDiff'`. | Yes      |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the trace\_replayBlockTransactions **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "trace_replayBlockTransactions",
    "params": ["latest", ["trace"]],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "trace_replayBlockTransactions", "params": ["latest", ["trace"]], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "output": "0x...",
            "stateDiff": null,
            "trace": [...],
            "vmTrace": null,
            "transactionHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
        }
    ]
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: An array of replay results with requested trace types for each transaction.

### Use Case

The trace\_replayBlockTransactions method is commonly used for:

* **Block replay analysis**
* **State change tracking**
* **VM execution analysis**
* **Historical debugging**

### Code Example

Here's how to use the trace\_replayBlockTransactions method with different programming languages:

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
    "method": "trace_replayBlockTransactions",
    "params": ["latest", ["trace"]],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("trace_replayBlockTransactions result:", result)
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
    method: "trace_replayBlockTransactions",
    params: ["latest", ["trace"]],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("trace_replayBlockTransactions result:", response.data.result);
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
