---
description: >-
  Example code for the eth_getBlockTransactionCountByHash JSON-RPC method.
  Complete guide on how to use eth_getBlockTransactionCountByHash JSON-RPC in
  GetBlock Web3 documentation.
---

# eth\_getblocktransactioncountbyhash - Worldchain

Returns the number of transactions in a block specified by block hash on the World Chain network.

### Parameters

| Parameter | Type   | Description                  |
| --------- | ------ | ---------------------------- |
| blockHash | string | Hash of the block (32 bytes) |

### Request

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockTransactionCountByHash",
    "params": ["0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getBlockTransactionCountByHash",
    "params": ["0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e"],
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
    "method": "eth_getBlockTransactionCountByHash",
    "params": ["0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e"],
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
            "method": "eth_getBlockTransactionCountByHash",
            "params": ["0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e"],
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
    "result": "0x10"
}
```

### Response Parameters

| Field  | Type   | Description                                                           |
| ------ | ------ | --------------------------------------------------------------------- |
| result | string | Hexadecimal representation of the number of transactions in the block |

### Use Case

The `eth_getBlockTransactionCountByHash` method on World Chain is typically used for:

* Block analysis
* Transaction counting
* Network activity monitoring
* Block validation

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

const count = await provider.send('eth_getBlockTransactionCountByHash', ['0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e']);
console.log('Transaction Count:', parseInt(count, 16));
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

const count = await client.getBlockTransactionCount({
    blockHash: '0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e'
});
console.log('Transaction Count:', count);
```
{% endtab %}
{% endtabs %}
