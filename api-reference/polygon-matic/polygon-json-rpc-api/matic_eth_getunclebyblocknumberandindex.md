---
description: >-
  Example code for the eth_getUncleByBlockHashAndIndex json-rpc method. Сomplete
  guide on how to use eth_getUncleByBlockHashAndIndex json-rpc in GetBlock.io
  Web3 documentation.
---

# eth\_getUncleByBlockHashAndIndex - Polygon

The **eth\_getUncleByBlockHashAndIndex** method returns information about an uncle of a block by hash and uncle index position. Note that Polygon uses Proof of Stake, so uncle blocks are rare.

## Parameters

| Parameter  | Type   | Required | Description                         |
| ---------- | ------ | -------- | ----------------------------------- |
| blockHash  | string | Yes      | 32-byte hash of the block           |
| uncleIndex | string | Yes      | Uncle index position in hexadecimal |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getUncleByBlockHashAndIndex",
    "params": ["0x3f07a9c83155594c000642e7d60e8a8a00038d03e9849171a05ed0e2d47acbb3", "0x0"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_getUncleByBlockHashAndIndex',
    params: ["0x3f07a9c83155594c000642e7d60e8a8a00038d03e9849171a05ed0e2d47acbb3", "0x0"],
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
{% code title="example.py" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getUncleByBlockHashAndIndex",
    "params": ["0x3f07a9c83155594c000642e7d60e8a8a00038d03e9849171a05ed0e2d47acbb3", "0x0"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
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
            "method": "eth_getUncleByBlockHashAndIndex",
            "params": ["0x3f07a9c83155594c000642e7d60e8a8a00038d03e9849171a05ed0e2d47acbb3", "0x0"],
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
    "result": null
}
```

## Response Parameters

| Field   | Type   | Description                             |
| ------- | ------ | --------------------------------------- |
| jsonrpc | string | JSON-RPC version (2.0)                  |
| id      | string | Request identifier                      |
| result  | varies | Uncle block object or null if not found |

## Use Case

The eth\_getUncleByBlockHashAndIndex method is useful for:

* **Ethereum compatibility**
* **Chain analysis**
* **Historical data**

## Error Handling

| Status Code | Error Message   | Cause                           |
| ----------- | --------------- | ------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN |
| -32600      | Invalid Request | Malformed request body          |
| -32602      | Invalid params  | Invalid method parameters       |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_getUncleByBlockHashAndIndex', ["0x3f07a9c83155594c000642e7d60e8a8a00038d03e9849171a05ed0e2d47acbb3", "0x0"]);
console.log('Result:', result);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { polygon } from 'viem/chains';

const client = createPublicClient({
    chain: polygon,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({
    method: 'eth_getUncleByBlockHashAndIndex',
    params: ["0x3f07a9c83155594c000642e7d60e8a8a00038d03e9849171a05ed0e2d47acbb3", "0x0"]
});
console.log('Result:', result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
