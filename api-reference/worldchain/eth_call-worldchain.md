---
description: >-
  Example code for the eth_call JSON-RPC method. Complete guide on how to use
  eth_call JSON-RPC in GetBlock Web3 documentation.
---

# eth\_call - Worldchain

This method executes a new message call immediately without creating a transaction on the World Chain blockchain. This is commonly used to read data from smart contracts.

### Parameters

| Parameter   | Type   | Description                                                                    |
| ----------- | ------ | ------------------------------------------------------------------------------ |
| transaction | object | Transaction call object with to, data, and optional from, gas, gasPrice, value |
| blockNumber | string | Block number in hex, or "latest", "earliest", "pending"                        |

### Request

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{"to": "0x1234567890123456789012345678901234567890", "data": "0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc454e4438f44e"}, "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{"to": "0x1234567890123456789012345678901234567890", "data": "0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc454e4438f44e"}, "latest"],
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
    "method": "eth_call",
    "params": [{"to": "0x1234567890123456789012345678901234567890", "data": "0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc454e4438f44e"}, "latest"],
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
            "method": "eth_call",
            "params": [{"to": "0x1234567890123456789012345678901234567890", "data": "0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc454e4438f44e"}, "latest"],
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
    "result": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000"
}
```

### Response Parameters

| Field  | Type   | Description                                      |
| ------ | ------ | ------------------------------------------------ |
| result | string | The return value of the executed contract method |

### Use Case

The `eth_call` method on World Chain is typically used for:

* Reading contract state
* Token balance queries
* Contract function simulation
* Data retrieval

### Error Handling

| Status Code | Error Message | Cause                           |
| ----------- | ------------- | ------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.call({
    to: '0x1234567890123456789012345678901234567890',
    data: '0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc454e4438f44e'
});
console.log('Result:', result);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { worldchain } from 'viem/chains';

const client = createPublicClient({
    chain: worldchain,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.call({
    to: '0x1234567890123456789012345678901234567890',
    data: '0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc454e4438f44e'
});
console.log('Result:', result);
```
{% endtab %}
{% endtabs %}
