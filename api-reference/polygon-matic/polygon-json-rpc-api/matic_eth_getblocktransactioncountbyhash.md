---
description: >-
  Example code for the eth_getBlockTransactionCountByHash json-rpc method.
  Сomplete guide on how to use eth_getBlockTransactionCountByHash json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_getBlockTransactionCountByHash - Polygon

The **eth\_getBlockTransactionCountByHash** method returns the number of transactions in a block matching the given block hash.

## Parameters

| Parameter | Type   | Required | Description               |
| --------- | ------ | -------- | ------------------------- |
| blockHash | string | Yes      | 32-byte hash of the block |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockTransactionCountByHash",
    "params": ["0x3f07a9c83155594c000642e7d60e8a8a00038d03e9849171a05ed0e2d47acbb3"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_getBlockTransactionCountByHash',
    params: ["0x3f07a9c83155594c000642e7d60e8a8a00038d03e9849171a05ed0e2d47acbb3"],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python (requests)" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getBlockTransactionCountByHash",
    "params": ["0x3f07a9c83155594c000642e7d60e8a8a00038d03e9849171a05ed0e2d47acbb3"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
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
            "method": "eth_getBlockTransactionCountByHash",
            "params": ["0x3f07a9c83155594c000642e7d60e8a8a00038d03e9849171a05ed0e2d47acbb3"],
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

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x8f"
}
```

## Response Parameters

| Field   | Type   | Description                                        |
| ------- | ------ | -------------------------------------------------- |
| jsonrpc | string | JSON-RPC version (2.0)                             |
| id      | string | Request identifier                                 |
| result  | varies | Number of transactions in the block as hexadecimal |

## Use Case

The eth\_getBlockTransactionCountByHash method is useful for:

* **Network activity monitoring**
* **Block analysis**
* **Transaction counting**
* **Statistics gathering**

## Error Handling

| Status Code | Error Message   | Cause                           |
| ----------- | --------------- | ------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN |
| -32600      | Invalid Request | Malformed request body          |
| -32602      | Invalid params  | Invalid method parameters       |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_getBlockTransactionCountByHash', ["0x3f07a9c83155594c000642e7d60e8a8a00038d03e9849171a05ed0e2d47acbb3"]);
console.log('Result:', result);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { polygon } from 'viem/chains';

const client = createPublicClient({
    chain: polygon,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({
    method: 'eth_getBlockTransactionCountByHash',
    params: ["0x3f07a9c83155594c000642e7d60e8a8a00038d03e9849171a05ed0e2d47acbb3"]
});
console.log('Result:', result);
```
{% endtab %}
{% endtabs %}
