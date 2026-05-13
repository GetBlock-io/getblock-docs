# trace\_transaction

{% hint style="success" %}
The **trace\_transaction** method returns detailed execution traces for a specific transaction on Optimism.
{% endhint %}

This method returns the complete execution trace of a transaction, including all internal calls, contract creations, and value transfers. Each trace includes the call type, addresses involved, input/output data, and gas usage. This is invaluable for debugging and understanding complex contract interactions.

### Supported Networks

The trace\_transaction **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
One parameter is required:
{% endhint %}

| Parameter       | Type           | Description           | Required |
| --------------- | -------------- | --------------------- | -------- |
| transactionHash | DATA, 32 Bytes | The transaction hash. | Yes      |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the trace\_transaction **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "trace_transaction",
    "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "trace_transaction", "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"], "id": "getblock.io"}
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
                "gas": "0x76c0",
                "input": "0x...",
                "value": "0x0"
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
* **`result`**: An array of trace objects showing all execution steps in the transaction.

### Use Case

The trace\_transaction method is commonly used for:

* **Transaction debugging**
* **Internal call analysis**
* **Security auditing**
* **Gas optimization analysis**

### Code Example

Here's how to use the trace\_transaction method with different programming languages:

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
    "method": "trace_transaction",
    "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("trace_transaction result:", result)
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
    method: "trace_transaction",
    params: ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("trace_transaction result:", response.data.result);
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
