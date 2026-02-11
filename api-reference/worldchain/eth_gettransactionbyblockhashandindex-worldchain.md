---
description: >-
  Example code for the eth_getTransactionByBlockHashAndIndex JSON-RPC method.
  Complete guide on how to use eth_getTransactionByBlockHashAndIndex JSON-RPC in
  GetBlock Web3 documentation.
---

# eth\_gettransactionbyblockhashandindex - Worldchain

This method returns information about a transaction by block hash and transaction index position on the World Chain network.

### Parameters

| Parameter        | Type   | Description                      |
| ---------------- | ------ | -------------------------------- |
| blockHash        | string | Hash of the block (32 bytes)     |
| transactionIndex | string | Transaction index position (hex) |

### Request

{% tabs %}
{% tab title="curl" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockHashAndIndex",
    "params": ["0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e", "0x0"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockHashAndIndex",
    "params": ["0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e", "0x0"],
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
    "method": "eth_getTransactionByBlockHashAndIndex",
    "params": ["0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e", "0x0"],
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
            "method": "eth_getTransactionByBlockHashAndIndex",
            "params": ["0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e", "0x0"],
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
    "result": {
        "hash": "0x...",
        "blockNumber": "0x1A5C3B2",
        "from": "0x...",
        "to": "0x...",
        "value": "0x..."
    }
}
```

### Response Parameters

| Field       | Type   | Description              |
| ----------- | ------ | ------------------------ |
| hash        | string | Transaction hash         |
| blockNumber | string | Block number             |
| from        | string | Sender address           |
| to          | string | Recipient address        |
| value       | string | Value transferred in wei |

### Use Case

The `eth_getTransactionByBlockHashAndIndex` method on World Chain is typically used for:

* Block transaction retrieval
* Index-based lookup
* Block analysis
* Transaction iteration

### Error Handling

| Status Code | Error Message | Cause                           |
| ----------- | ------------- | ------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const tx = await provider.send('eth_getTransactionByBlockHashAndIndex', ['0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e', '0x0']);
console.log('Transaction:', tx);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { worldchain } from 'viem/chains';

const client = createPublicClient({
    chain: worldchain,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const tx = await client.request({
    method: 'eth_getTransactionByBlockHashAndIndex',
    params: ['0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e', '0x0']
});
console.log('Transaction:', tx);
```
{% endtab %}
{% endtabs %}
