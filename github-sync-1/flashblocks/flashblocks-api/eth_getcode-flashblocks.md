---
description: >-
  Example code for the eth_getCode JSON-RPC method. Complete guide on how to use
  eth_getCode JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getCode - Flashblocks

The `eth_getCode` method returns the bytecode at a given address. This is used to determine if an address is a smart contract and to retrieve the deployed contract code. When the block reference is `"pending"`, Flashblocks detect contract deployments before the block seals.

## Parameters

| Parameter      | Type   | Required | Description                         |
| -------------- | ------ | -------- | ----------------------------------- |
| address        | string | Yes      | 20-byte address to check            |
| blockParameter | string | Yes      | Block number in hex, or "`pending`" |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": [
    "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
    "pending",
    ]
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios (JavaScript)" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getCode',
    params: [
    '0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913',
    'pending'],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const code = response.data.result;
const isContract = code !== '0x';
console.log('Is Contract:', isContract);
console.log('Code Length:', (code.length - 2) / 2, 'bytes');
```
{% endtab %}

{% tab title="Requests (Python)" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getCode',
        'params': [
    '0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913',
    'pending'],
        'id': 'getblock.io'
    }
)

result = response.json()
code = result['result']
is_contract = code != '0x'
print(f'Is Contract: {is_contract}')
print(f'Code Length: {(len(code) - 2) // 2} bytes')
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getCode",
            "params": [
    "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
    "pending"],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    let code = response["result"].as_str().unwrap();
    let is_contract = code != "0x";
    println!("Is Contract: {}", is_contract);
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
    "result": "0x608060405234801561001057600080fd5b50..."
}
```

## Response Parameters

| Parameter | Type   | Description                                          |
| --------- | ------ | ---------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                    |
| id        | string | Request identifier matching the request              |
| result    | string | Bytecode at the address (hex). Returns "0x" for EOAs |

## Use Cases

* Contract Detection: Verify if address is a smart contract
* Security Analysis: Examine contract bytecode for vulnerabilities
* Contract Verification: Compare deployed code with source
* Proxy Detection: Identify proxy patterns in bytecode
* Contract Indexing: Catalog deployed contracts

## Error Handling

| Error Code | Message        | Description            |
| ---------- | -------------- | ---------------------- |
| -32602     | Invalid params | Invalid address format |
| -32603     | Internal error | Node internal failure  |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call — 'pending' returns Flashblocks-preconfirmed state:
const result = await provider.send('eth_code', [
    "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
    "pending"]);
console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
{% code overflow="wrap" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/')
});

// viem's read methods accept blockTag: 'pending' for Flashblocks-preconfirmed state:
const result = await client.request({
    method: 'eth_code',
    params: [
    "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
    "pending"]
});
console.log(result);

// Typed wrappers also accept blockTag: 'pending':
// client.getBlock({ blockTag: 'pending' })
```
{% endcode %}
{% endtab %}
{% endtabs %}
