---
description: >-
  Example code for the eth_getTransactionByBlockHashAndIndex json-rpc method.
  Сomplete guide on how to use eth_getTransactionByBlockHashAndIndex json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_getTransactionByBlockHashAndIndex - Polygon

The `eth_getTransactionByBlockHashAndIndex` method returns information about a transaction by block hash and transaction index position.

## Parameters

| Parameter        | Type   | Required | Description                               |
| ---------------- | ------ | -------- | ----------------------------------------- |
| blockHash        | string | Yes      | 32-byte hash of the block                 |
| transactionIndex | string | Yes      | Transaction index position in hexadecimal |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockHashAndIndex",
    "params": ["0x3f07a9c83155594c000642e7d60e8a8a00038d03e9849171a05ed0e2d47acbb3", "0x0"],
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
    method: 'eth_getTransactionByBlockHashAndIndex',
    params: ["0x3f07a9c83155594c000642e7d60e8a8a00038d03e9849171a05ed0e2d47acbb3", "0x0"],
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
    "method": "eth_getTransactionByBlockHashAndIndex",
    "params": ["0x3f07a9c83155594c000642e7d60e8a8a00038d03e9849171a05ed0e2d47acbb3", "0x0"],
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
            "method": "eth_getTransactionByBlockHashAndIndex",
            "params": ["0x3f07a9c83155594c000642e7d60e8a8a00038d03e9849171a05ed0e2d47acbb3", "0x0"],
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
    "result": {
        "hash": "0x...",
        "blockHash": "0x...",
        "blockNumber": "0x...",
        "from": "0x...",
        "to": "0x...",
        "value": "0x..."
    }
}
```

<details>

<summary>Response Parameters</summary>

| Field   | Type   | Description                             |
| ------- | ------ | --------------------------------------- |
| jsonrpc | string | JSON-RPC version (2.0)                  |
| id      | string | Request identifier                      |
| result  | varies | Transaction object or null if not found |

</details>

## Use Case

The `eth_getTransactionByBlockHashAndIndex` method is useful for:

* Block scanning
* Transaction indexing
* Block explorers
* Data extraction

## Error Handling

| Status Code | Error Message   | Cause                           |
| ----------- | --------------- | ------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN |
| -32600      | Invalid Request | Malformed request body          |
| -32602      | Invalid params  | Invalid method parameters       |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_getTransactionByBlockHashAndIndex', ["0x3f07a9c83155594c000642e7d60e8a8a00038d03e9849171a05ed0e2d47acbb3", "0x0"]);
console.log('Result:', result);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code overflow="wrap" %}
```javascript
import { createPublicClient, http } from 'viem';
import { polygon } from 'viem/chains';

const client = createPublicClient({
    chain: polygon,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({
    method: 'eth_getTransactionByBlockHashAndIndex',
    params: ["0x3f07a9c83155594c000642e7d60e8a8a00038d03e9849171a05ed0e2d47acbb3", "0x0"]
});
console.log('Result:', result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
