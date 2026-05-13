# eth\_newFilter

{% hint style="success" %}
The **eth\_newFilter** method creates a new filter for receiving log events on Optimism that match specified criteria.
{% endhint %}

This method creates a filter that can be polled for new log events matching the specified criteria. The filter can be configured with address, topics, and block range parameters. Once created, use eth\_getFilterChanges to retrieve new logs that match the filter. Filters timeout if not polled regularly.

### Supported Networks

The eth\_newFilter **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
One parameter is required:
{% endhint %}

| Parameter    | Type   | Description                                                                 | Required |
| ------------ | ------ | --------------------------------------------------------------------------- | -------- |
| filterObject | Object | Filter object with optional fromBlock, toBlock, address, and topics fields. | Yes      |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_newFilter **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_newFilter",
    "params": [{"fromBlock": "latest", "address": "0x4200000000000000000000000000000000000006", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_newFilter", "params": [{"fromBlock": "latest", "address": "0x4200000000000000000000000000000000000006", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x1"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: A filter ID that can be used with eth\_getFilterChanges and eth\_uninstallFilter.

### Use Case

The eth\_newFilter method is commonly used for:

* **Event monitoring**
* **Log polling**
* **Real-time event tracking**
* **Contract event subscription**

### Code Example

Here's how to use the eth\_newFilter method with different programming languages:

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
    "method": "eth_newFilter",
    "params": [{"fromBlock": "latest", "address": "0x4200000000000000000000000000000000000006", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_newFilter result:", result)
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
    method: "eth_newFilter",
    params: [{"fromBlock": "latest", "address": "0x4200000000000000000000000000000000000006", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_newFilter result:", response.data.result);
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
