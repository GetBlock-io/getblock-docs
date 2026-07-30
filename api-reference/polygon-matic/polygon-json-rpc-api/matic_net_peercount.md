---
description: >-
  Example code for the net_peerCount json-rpc method. Сomplete guide on how to
  use net_peerCount json-rpc in GetBlock.io Web3 documentation.
---

# net\_peerCount - Polygon

The **net\_peerCount** method returns the number of peers currently connected to the client.

## Parameters

* None

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "net_peerCount",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'net_peerCount',
    params: [],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endcode %}


{% endtab %}

{% tab title="Request" %}
{% code title="Python (requests)" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "net_peerCount",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust (reqwest)" %}
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
            "method": "net_peerCount",
            "params": [],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x19"
}
```

## Response Parameters

| Field   | Type   | Description                              |
| ------- | ------ | ---------------------------------------- |
| jsonrpc | string | JSON-RPC version (2.0)                   |
| id      | string | Request identifier                       |
| result  | varies | Number of connected peers as hexadecimal |

## Use Case

The net\_peerCount method is useful for:

* **Node health check**
* **Connectivity assessment**

## Error Handling

| Status Code | Error Message   | Cause                           |
| ----------- | --------------- | ------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN |
| -32600      | Invalid Request | Malformed request body          |
| -32602      | Invalid params  | Invalid method parameters       |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('net_peerCount', []);
console.log('Result:', result);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { polygon } from 'viem/chains';

const client = createPublicClient({
    chain: polygon,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({
    method: 'net_peerCount',
    params: []
});
console.log('Result:', result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
