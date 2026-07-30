---
description: >-
  Example code for the eth_getTransactionByBlockNumberAndIndex json-rpc method.
  Сomplete guide on how to use eth_getTransactionByBlockNumberAndIndex json-rpc
  in GetBlock.io Web3 documentation.
---

# eth\_getTransactionByBlockNumberAndIndex - Polygon

The **eth\_getTransactionByBlockNumberAndIndex** method returns information about a transaction by block number and transaction index position.

## Parameters

| Parameter        | Type   | Required | Description                                             |
| ---------------- | ------ | -------- | ------------------------------------------------------- |
| blockNumber      | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |
| transactionIndex | string | Yes      | Transaction index position in hexadecimal               |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockNumberAndIndex",
    "params": ["0x3A2B5C7", "0x0"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_getTransactionByBlockNumberAndIndex',
    params: ["0x3A2B5C7", "0x0"],
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

{% tab title="Python (requests)" %}
{% code title="example.py" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockNumberAndIndex",
    "params": ["0x3A2B5C7", "0x0"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust (reqwest)" %}
{% code title="main.rs" %}
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
            "method": "eth_getTransactionByBlockNumberAndIndex",
            "params": ["0x3A2B5C7", "0x0"],
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

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "hash": "0x...",
        "blockNumber": "0x3A2B5C7",
        "from": "0x...",
        "to": "0x...",
        "value": "0x..."
    }
}
```
{% endcode %}

## Response Parameters

| Field   | Type   | Description                             |
| ------- | ------ | --------------------------------------- |
| jsonrpc | string | JSON-RPC version (2.0)                  |
| id      | string | Request identifier                      |
| result  | varies | Transaction object or null if not found |

## Use Case

The eth\_getTransactionByBlockNumberAndIndex method is useful for:

* **Sequential block scanning**
* **Transaction indexing**
* **Historical data extraction**
* **Block explorer functionality**

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

const result = await provider.send('eth_getTransactionByBlockNumberAndIndex', ["0x3A2B5C7", "0x0"]);
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
    method: 'eth_getTransactionByBlockNumberAndIndex',
    params: ["0x3A2B5C7", "0x0"]
});
console.log('Result:', result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
