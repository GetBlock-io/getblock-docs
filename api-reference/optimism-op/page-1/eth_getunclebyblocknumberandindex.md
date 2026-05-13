# eth\_getUncleByBlockNumberAndIndex

{% hint style="success" %}
The **eth\_getUncleByBlockNumberAndIndex** method returns information about an uncle block by number on Optimism (typically null).
{% endhint %}

This method returns uncle block information by block number. Like eth\_getUncleByBlockHashAndIndex, this typically returns null on Optimism since the network doesn't have traditional uncles.

### Supported Networks

The eth\_getUncleByBlockNumberAndIndex **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
Two parameters are required:
{% endhint %}

| Parameter   | Type            | Description                                       | Required |
| ----------- | --------------- | ------------------------------------------------- | -------- |
| blockNumber | `QUANTITY\|TAG` | Block number in hex, or `'latest'`, `'earliest'`. | Yes      |
| uncleIndex  | `QUANTITY`      | The uncle's index position in hex.                | Yes      |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_getUncleByBlockNumberAndIndex **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getUncleByBlockNumberAndIndex",
    "params": ["latest", "0x0"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_getUncleByBlockNumberAndIndex", "params": ["latest", "0x0"], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": null
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: Uncle block object or null (typically null on Optimism).

### Use Case

The eth\_getUncleByBlockNumberAndIndex method is commonly used for:

* **Ethereum compatibility**
* **API completeness**

### Code Example

Here's how to use the eth\_getUncleByBlockNumberAndIndex method with different programming languages:

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
    "method": "eth_getUncleByBlockNumberAndIndex",
    "params": ["latest", "0x0"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getUncleByBlockNumberAndIndex result:", result)
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
    method: "eth_getUncleByBlockNumberAndIndex",
    params: ["latest", "0x0"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getUncleByBlockNumberAndIndex result:", response.data.result);
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
