---
description: >-
  Example code for the eth_getBlockByHash JSON-RPC method. Complete guide on how
  to use eth_getBlockByHash JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getblockbyhash - Worldchain

Returns information about a block by hash on the World Chain network.

### Parameters

| Parameter        | Type    | Description                                                                     |
| ---------------- | ------- | ------------------------------------------------------------------------------- |
| blockHash        | string  | Hash of the block (32 bytes)                                                    |
| fullTransactions | boolean | If true, returns full transaction objects; if false, returns transaction hashes |

### Request

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": ["0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e", false],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": ["0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e", false],
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
    "method": "eth_getBlockByHash",
    "params": ["0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e", false],
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
            "method": "eth_getBlockByHash",
            "params": ["0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e", false],
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
        "number": "0x1A5C3B2",
        "hash": "0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e",
        "parentHash": "0x...",
        "timestamp": "0x...",
        "transactions": [...]
    }
}
```

### Response Parameters

| Field        | Type   | Description                            |
| ------------ | ------ | -------------------------------------- |
| number       | string | Block number in hex                    |
| hash         | string | Block hash                             |
| parentHash   | string | Hash of the parent block               |
| timestamp    | string | Block timestamp                        |
| transactions | array  | Array of transaction objects or hashes |

### Use Case

The `eth_getBlockByHash` method on World Chain is typically used for:

* Block data retrieval
* Transaction history
* Chain analysis
* Block verification

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

const block = await provider.getBlock('0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e');
console.log('Block:', block);
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

const block = await client.getBlock({
    blockHash: '0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e'
});
console.log('Block:', block);
```
{% endtab %}
{% endtabs %}
