# eth\_call

{% hint style="success" %}
The **eth\_call** method executes a read-only call to a smart contract on Optimism without consuming gas or modifying state.
{% endhint %}

This method allows you to interact with smart contracts without sending a transaction or spending gas. It's commonly used to read data from contracts, such as checking token balances or calling view functions. The call is executed against the state at the specified block. This is essential for any application that needs to read contract data without modifying the blockchain state.

### Supported Networks

The eth\_call **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
Two parameters are required:
{% endhint %}

| Parameter   | Type            | Description                                                                                                               | Required |
| ----------- | --------------- | ------------------------------------------------------------------------------------------------------------------------- | -------- |
| transaction | Object          | Transaction call object with from (optional), to, gas (optional), gasPrice (optional), value (optional), and data fields. | Yes      |
| block       | `QUANTITY\|TAG` | Block number in hex, or `'latest'`, `'earliest'`, `'pending'`.                                                            | Yes      |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_call **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{"to": "0x4200000000000000000000000000000000000006", "data": "0x70a08231000000000000000000000000742d35Cc6634C0532925a3b844Bc9e7595f7bD5e"}, "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_call", "params": [{"to": "0x4200000000000000000000000000000000000006", "data": "0x70a08231000000000000000000000000742d35Cc6634C0532925a3b844Bc9e7595f7bD5e"}, "latest"], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x0000000000000000000000000000000000000000000000000000000005f5e100"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The return value of the executed contract call as a hexadecimal string.

### Use Case

The eth\_call method is commonly used for:

* **Reading token balances**
* **Calling view functions**
* **Contract state inspection**
* **Simulating transactions**

### Code Example

Here's how to use the eth\_call method with different programming languages:

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
    "method": "eth_call",
    "params": [{"to": "0x4200000000000000000000000000000000000006", "data": "0x70a08231000000000000000000000000742d35Cc6634C0532925a3b844Bc9e7595f7bD5e"}, "latest"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_call result:", result)
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
    method: "eth_call",
    params: [{"to": "0x4200000000000000000000000000000000000006", "data": "0x70a08231000000000000000000000000742d35Cc6634C0532925a3b844Bc9e7595f7bD5e"}, "latest"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_call result:", response.data.result);
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
