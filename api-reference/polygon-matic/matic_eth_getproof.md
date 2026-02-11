---
description: >-
  Example code for the eth_getProof json-rpc method. Сomplete guide on how to
  use eth_getProof json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getProof - Polygon

The `eth_getProof` method returns the Merkle proof for a given account, including the account balance, nonce, code hash, storage hash, and storage proofs for specified storage keys.

## Parameters

| Parameter      | Type   | Required | Description                                                   |
| -------------- | ------ | -------- | ------------------------------------------------------------- |
| address        | string | Yes      | 20-byte account address                                       |
| storageKeys    | array  | Yes      | Array of 32-byte storage keys to prove                        |
| blockParameter | string | Yes      | Block number in hex, or `"latest"`, `"earliest"`, `"pending"` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": ["0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0", ["0x0"], "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_getProof',
    params: ["0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0", ["0x0"], "latest"],
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
{% code title="request.py" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": ["0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0", ["0x0"], "latest"],
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
            "method": "eth_getProof",
            "params": ["0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0", ["0x0"], "latest"],
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
        "address": "0x...",
        "accountProof": ["0x..."],
        "balance": "0x...",
        "codeHash": "0x...",
        "nonce": "0x...",
        "storageHash": "0x...",
        "storageProof": []
    }
}
```
{% endcode %}

## Response Parameters

| Field   | Type   | Description                             |
| ------- | ------ | --------------------------------------- |
| jsonrpc | string | JSON-RPC version (`2.0`)                |
| id      | string | Request identifier                      |
| result  | varies | Account proof object with Merkle proofs |

## Use Case

The `eth_getProof` method is useful for:

* State verification
* Light client support
* Cross-chain bridges
* Fraud proofs

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

const result = await provider.send('eth_getProof', ["0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0", ["0x0"], "latest"]);
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
    method: 'eth_getProof',
    params: ["0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0", ["0x0"], "latest"]
});
console.log('Result:', result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
