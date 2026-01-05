---
description: >-
  Example code for the eth_getFilterChanges JSON-RPC method. Complete guide on
  how to use eth_getFilterChanges JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getFilterChanges - Mantle

This method polls for filter changes. Returns an array of logs that occurred since last poll for log filters, or an array of block hashes for block filters.

### Parameters

| Parameter | Type   | Description                                                     |
| --------- | ------ | --------------------------------------------------------------- |
| filterId  | string | The filter ID returned by eth\_newFilter or eth\_newBlockFilter |

### Request

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getFilterChanges",
    "params": ["0x1a2b3c4d5e6f"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getFilterChanges",
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
{% endtab %}

{% tab title="Python (requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getFilterChanges",
    "params": ["0x1a2b3c4d5e6f"],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (reqwest)" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "eth_getFilterChanges",
            "params": ["0x1a2b3c4d5e6f"],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
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

| Field  | Type  | Description                                          |
| ------ | ----- | ---------------------------------------------------- |
| result | array | Array of log objects or block hashes since last poll |

### Use Case

The `eth_getFilterChanges` method is essential for:

* Polling for new events
* Block monitoring via HTTP
* Event-driven applications
* Backend services without WebSocket
* Incremental log retrieval

### Error Handling

| Status Code | Error Message    | Cause                           |
| ----------- | ---------------- | ------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS-TOKEN |
| -32000      | Filter not found | Invalid or expired filter ID    |

<details>

<summary>Errors and causes</summary>



</details>

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const changes = await provider.send('eth_getFilterChanges', ['0x1a2b3c4d5e6f']);
console.log('Filter Changes:', changes);
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

const changes = await client.request({
    method: 'eth_getFilterChanges',
    params: ['0x1a2b3c4d5e6f']
});
console.log('Filter Changes:', changes);
```
{% endtab %}
{% endtabs %}
