# eth\_unsubscribe

{% hint style="success" %}
The **eth\_unsubscribe** method cancels an existing WebSocket subscription on Optimism.
{% endhint %}

This method cancels a subscription that was created with `eth_subscribe`. Once cancelled, no more notifications will be sent for that subscription. This should be called when real-time updates are no longer needed.

### Supported Networks

The eth\_unsubscribe **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
One parameter is required:
{% endhint %}

| Parameter      | Type   | Description                    | Required |
| -------------- | ------ | ------------------------------ | -------- |
| subscriptionId | String | The subscription ID to cancel. | Yes      |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_unsubscribe **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_unsubscribe",
    "params": ["0x9ce59a13059e417087c02d3236a0b1cc"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_unsubscribe", "params": ["0x9ce59a13059e417087c02d3236a0b1cc"], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": true
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: Boolean indicating whether the subscription was successfully cancelled.

### Use Case

The eth\_unsubscribe method is commonly used for:

* **Subscription management**
* **Resource cleanup**
* **Connection lifecycle management**

### Code Example

Here's how to use the eth\_unsubscribe method with different programming languages:

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
    "method": "eth_unsubscribe",
    "params": ["0x9ce59a13059e417087c02d3236a0b1cc"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_unsubscribe result:", result)
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
    method: "eth_unsubscribe",
    params: ["0x9ce59a13059e417087c02d3236a0b1cc"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_unsubscribe result:", response.data.result);
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
