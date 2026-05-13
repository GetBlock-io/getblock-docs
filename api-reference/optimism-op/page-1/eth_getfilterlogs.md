# eth\_getFilterLogs

{% hint style="success" %}
The **eth\_getFilterLogs** method returns all logs that match the criteria of the specified filter on Optimism.
{% endhint %}

This method retrieves all logs that match a filter's criteria, not just changes since the last poll. This is useful when you need to get all historical logs matching a filter's criteria. Note that this only works with filters created by eth\_newFilter.

### Supported Networks

The eth\_getFilterLogs **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
One parameter is required:
{% endhint %}

| Parameter | Type     | Description                               | Required |
| --------- | -------- | ----------------------------------------- | -------- |
| filterId  | QUANTITY | The filter ID returned by eth\_newFilter. | Yes      |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_getFilterLogs **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getFilterLogs",
    "params": ["0x1"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_getFilterLogs", "params": ["0x1"], "id": "getblock.io"}
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
            "address": "0x4200000000000000000000000000000000000006",
            "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"],
            "data": "0x0000...",
            "blockNumber": "0x7A69B00",
            "transactionHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
            "logIndex": "0x0"
        }
    ]
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: An array of all log objects matching the filter criteria.

### Use Case

The eth\_getFilterLogs method is commonly used for:

* **Historical log retrieval**
* **Complete event history**
* **Filter result aggregation**

### Code Example

Here's how to use the eth\_getFilterLogs method with different programming languages:

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
    "method": "eth_getFilterLogs",
    "params": ["0x1"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getFilterLogs result:", result)
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
    method: "eth_getFilterLogs",
    params: ["0x1"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getFilterLogs result:", response.data.result);
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
