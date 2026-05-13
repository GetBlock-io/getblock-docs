# trace\_block

{% hint style="success" %}
The **trace\_block** method returns execution traces for all transactions in a specified block on Optimism.
{% endhint %}

This method returns detailed execution traces for every transaction in a block, showing all internal calls, value transfers, and contract interactions. This is essential for debugging, security analysis, and understanding complex transaction flows. Note: Trace methods may not be available on all GetBlock nodes.

### Supported Networks

The trace\_block **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
One parameter is required:
{% endhint %}

| Parameter   | Type          | Description                                   | Required |
| ----------- | ------------- | --------------------------------------------- | -------- |
| blockNumber | QUANTITY\|TAG | Block number in hex, or 'latest', 'earliest'. | Yes      |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the trace\_block **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "trace_block",
    "params": ["latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "trace_block", "params": ["latest"], "id": "getblock.io"}
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
            "action": {
                "callType": "call",
                "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e",
                "to": "0x8ba1f109551bD432803012645Ac136dff489ca2c",
                "value": "0xde0b6b3a7640000",
                "gas": "0x76c0",
                "input": "0x"
            },
            "result": {
                "gasUsed": "0x5208",
                "output": "0x"
            },
            "type": "call"
        }
    ]
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: An array of trace objects for all transactions in the block, each containing action details and results.

### Use Case

The trace\_block method is commonly used for:

* **Transaction debugging**
* **Security analysis**
* **Internal transaction tracking**
* **Block analysis**

### Code Example

Here's how to use the trace\_block method with different programming languages:

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
    "method": "trace_block",
    "params": ["latest"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("trace_block result:", result)
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
    method: "trace_block",
    params: ["latest"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("trace_block result:", response.data.result);
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
