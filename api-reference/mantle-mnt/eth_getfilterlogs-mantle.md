---
description: >-
  Example code for the eth_getFilterLogs JSON-RPC method. Complete guide on how
  to use eth_getFilterLogs JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getFilterLogs - Mantle

This method returns an array of all logs matching filter with given ID. Unlike eth\_getFilterChanges, this returns all matching logs, not just changes since last poll.

### Parameters

| Parameter | Type   | Description                              |
| --------- | ------ | ---------------------------------------- |
| filterId  | string | The filter ID returned by eth\_newFilter |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getFilterLogs",
    "params": ["0x1a2b3c4d5e6f"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getFilterLogs",
    "params": ["0x1a2b3c4d5e6f"],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": ["latest", true],
    })

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
           .io"
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": []
}
```

### Response Parameters

| Field  | Type  | Description                                  |
| ------ | ----- | -------------------------------------------- |
| result | array | Array of all log objects matching the filter |

### Use Case

The `eth_getFilterLogs` method is essential for:

* Retrieving all logs for a filter
* Historical event queries
* Complete log retrieval
* Data synchronization
* Event replay scenarios

### Error Handling

| Status Code | Error Message    | Cause                           |
| ----------- | ---------------- | ------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS-TOKEN |
| -32000      | Filter not found | Invalid or expired filter ID    |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const logs = await provider.send('eth_getFilterLogs', ['0x1a2b3c4d5e6f']);
console.log('Filter Logs:', logs);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const logs = await client.request({
    method: 'eth_getFilterLogs',
    params: ['0x1a2b3c4d5e6f']
});
console.log('Filter Logs:', logs);
```
{% endtab %}
{% endtabs %}
