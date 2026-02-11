---
description: >-
  Example code for the eth_getProof JSON RPC method. Сomplete guide on how to
  use eth_getProof JSON RPC in GetBlock Web3 documentation.
---

# eth\_getProof - BSC

This method returns the account and storage values of the specified account, including the Merkle proof on the BNB Smart Chain. This is useful for verifying state without trusting the RPC provider.

## Parameters

| Parameter   | Type   | Required | Description              |
| ----------- | ------ | -------- | ------------------------ |
| address     | string | Yes      | Account address          |
| storageKeys | array  | Yes      | Array of storage keys    |
| blockNumber | string | Yes      | Block number or "latest" |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": [
        "0x55d398326f99059fF775485246999027B3197955",
        ["0x0"],
        "latest"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_getProof',
    params: [
        '0x55d398326f99059fF775485246999027B3197955',
        ['0x0'],
        'latest'
    ],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(JSON.stringify(response.data, null, 2)))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": [
        "0x55d398326f99059fF775485246999027B3197955",
        ["0x0"],
        "latest"
    ],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_getProof",
        "params": [
            "0x55d398326f99059fF775485246999027B3197955",
            ["0x0"],
            "latest"
        ],
        "id": "getblock.io"
    });

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);
    
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "accountProof": ["0xf90211a0..."],
        "balance": "0x0",
        "codeHash": "0x...",
        "nonce": "0x1",
        "storageHash": "0x...",
        "storageProof": [{
            "key": "0x0",
            "value": "0x0",
            "proof": ["0xf90211a0..."]
        }]
    }
}
```

## Response Parameters

| Field        | Type   | Description          |
| ------------ | ------ | -------------------- |
| accountProof | array  | Array of proof nodes |
| balance      | string | Account balance      |
| codeHash     | string | Code hash            |
| nonce        | string | Account nonce        |
| storageHash  | string | Storage root hash    |
| storageProof | array  | Storage proofs       |

## Use Cases

* Verify account state without trust
* Light client implementations
* Cross-chain bridges
* State verification for rollups

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32602     | Invalid params |
| -32603     | Internal error |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const proof = await provider.send('eth_getProof', [
    '0x55d398326f99059fF775485246999027B3197955',
    ['0x0'],
    'latest'
]);
console.log(proof);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const proof = await client.getProof({
    address: '0x55d398326f99059fF775485246999027B3197955',
    storageKeys: ['0x0']
});
console.log(proof);
```
{% endtab %}
{% endtabs %}
