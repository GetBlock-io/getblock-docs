# eth\_subscribe

{% hint style="success" %}
The **eth\_subscribe** method creates a WebSocket subscription for real-time events on Optimism.
{% endhint %}

This method creates a subscription that pushes data to the client via WebSocket when events occur. Subscription types include `'newHeads'` for new blocks, `'logs'` for log events, `'newPendingTransactions'` for pending transactions, and `'syncing'` for sync status changes. This provides more efficient real-time updates compared to polling.

### Supported Networks

The eth\_subscribe **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
One or two parameters:
{% endhint %}

| Parameter        | Type   | Description                                                                               | Required |
| ---------------- | ------ | ----------------------------------------------------------------------------------------- | -------- |
| subscriptionType | String | Type of subscription: `'newHeads'`, `'logs'`, `'newPendingTransactions'`, or `'syncing'`. | Yes      |
| filterObject     | Object | (Optional, for `'logs'`) Filter object with address and topics.                           | No       |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_subscribe **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_subscribe",
    "params": ["newHeads"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_subscribe", "params": ["newHeads"], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x9ce59a13059e417087c02d3236a0b1cc"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: A subscription ID that will be included in all subscription notifications.

### Use Case

The eth\_subscribe method is commonly used for:

* **Real-time block updates**
* **Live event streaming**
* **Efficient chain monitoring**
* **WebSocket applications**

### Code Example

Here's how to use the eth\_subscribe method with different programming languages:

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
    "method": "eth_subscribe",
    "params": ["newHeads"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_subscribe result:", result)
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
    method: "eth_subscribe",
    params: ["newHeads"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_subscribe result:", response.data.result);
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
