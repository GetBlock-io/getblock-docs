---
description: >-
  Example code for the eth_getProof JSON-RPC method. Complete guide on how to
  use eth_getProof JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getproof - Worldchain

Returns the account and storage values of the specified account including the Merkle-proof on the World Chain network.

### Parameters

| Parameter   | Type   | Description                                             |
| ----------- | ------ | ------------------------------------------------------- |
| address     | string | Address of the account                                  |
| storageKeys | array  | Array of storage keys to prove                          |
| blockNumber | string | Block number in hex, or "latest", "earliest", "pending" |

### Request

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": ["0x1234567890123456789012345678901234567890", ["0x0"], "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": ["0x1234567890123456789012345678901234567890", ["0x0"], "latest"],
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
    "method": "eth_getProof",
    "params": ["0x1234567890123456789012345678901234567890", ["0x0"], "latest"],
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
            "method": "eth_getProof",
            "params": ["0x1234567890123456789012345678901234567890", ["0x0"], "latest"],
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
        "address": "0x1234567890123456789012345678901234567890",
        "accountProof": ["0x..."],
        "balance": "0x0",
        "codeHash": "0x...",
        "nonce": "0x0",
        "storageHash": "0x...",
        "storageProof": [...]
    }
}
```

### Response Parameters

| Field        | Type   | Description                                         |
| ------------ | ------ | --------------------------------------------------- |
| address      | string | Address of the account                              |
| accountProof | array  | Array of RLP-serialized Merkle-Patricia proof nodes |
| balance      | string | Balance of the account                              |
| storageProof | array  | Array of storage proof objects                      |

### Use Case

The `eth_getProof` method on World Chain is typically used for:

* State verification
* Light client support
* Cross-chain bridges
* Proof generation

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

const proof = await provider.send('eth_getProof', ['0x1234567890123456789012345678901234567890', ['0x0'], 'latest']);
console.log('Proof:', proof);
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

const proof = await client.getProof({
    address: '0x1234567890123456789012345678901234567890',
    storageKeys: ['0x0']
});
console.log('Proof:', proof);
```
{% endtab %}
{% endtabs %}
