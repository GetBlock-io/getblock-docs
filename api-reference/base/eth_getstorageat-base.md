---
description: >-
  Example code for the eth_getStorageAt JSON-RPC method. Complete guide on how
  to use eth_getStorageAt JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getStorageAt - Base

The `eth_getStorageAt` method returns the value from a storage position at a given address. This is useful for reading raw contract storage slots, which is essential for analyzing contract state and proxy implementations.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| address        | string | Yes      | 20-byte address of the contract                         |
| position       | string | Yes      | Storage position (hex)                                  |
| blockParameter | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getStorageAt",
    "params": ["0x4200000000000000000000000000000000000006", "0x0", "latest"],
    "id": "getblock.io"
}'
```


{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getStorageAt',
    params: [
        '0x4200000000000000000000000000000000000006',
        '0x0',
        'latest'
    ],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log('Storage Value:', response.data.result);
```
{% endtab %}

{% tab title="Request" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getStorageAt',
        'params': [
            '0x4200000000000000000000000000000000000006',
            '0x0',
            'latest'
        ],
        'id': 'getblock.io'
    }
)

result = response.json()
print(f'Storage Value: {result["result"]}')
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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getStorageAt",
            "params": [
                "0x4200000000000000000000000000000000000006",
                "0x0",
                "latest"
            ],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Storage Value: {}", response["result"]);
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
    "result": "0x0000000000000000000000000000000000000000000000000000000000000000"
}
```

## Response Parameters

| Parameter | Type   | Description                                 |
| --------- | ------ | ------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")           |
| id        | string | Request identifier matching the request     |
| result    | string | 32-byte value at the storage position (hex) |

## Use Cases

* Proxy Detection: Read implementation address from proxy contracts
* Contract Analysis: Inspect raw contract state
* Token Analysis: Read token supply and holder data
* Governance: Check voting power and delegation
* Security Auditing: Verify contract storage layout

## Error Handling

| Error Code | Message        | Description                        |
| ---------- | -------------- | ---------------------------------- |
| -32602     | Invalid params | Invalid address or position format |
| -32603     | Internal error | Node internal failure              |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getStorageAt(address, slot) {
    const value = await provider.getStorage(address, slot);
    console.log('Storage Value:', value);
    return value;
}

// Read slot 0
getStorageAt('0x4200000000000000000000000000000000000006', 0);

// Read EIP-1967 implementation slot (for proxy contracts)
async function getProxyImplementation(proxyAddress) {
    // EIP-1967 implementation slot
    const implSlot = '0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc';
    const value = await provider.getStorage(proxyAddress, implSlot);
    const implAddress = '0x' + value.slice(26); // Extract address from 32-byte value
    console.log('Implementation:', implAddress);
    return implAddress;
}

// Calculate storage slot for mapping
function getMappingSlot(mappingSlot, key) {
    const paddedKey = ethers.zeroPadValue(key, 32);
    const paddedSlot = ethers.zeroPadValue(ethers.toBeHex(mappingSlot), 32);
    return ethers.keccak256(paddedKey + paddedSlot.slice(2));
}
```
{% endtab %}

{% tab title="Second Tab" %}
```javascript
import { createPublicClient, http, keccak256, pad, toHex, concat } from 'viem';
import { base } from 'viem/chains';

const client = createPublicClient({
    chain: base,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

async function getStorageAt(address, slot) {
    const value = await client.getStorageAt({
        address: address,
        slot: toHex(slot, { size: 32 })
    });
    
    console.log('Storage Value:', value);
    return value;
}

// Read slot 0
getStorageAt('0x4200000000000000000000000000000000000006', 0n);

// Read EIP-1967 implementation slot
async function getProxyImplementation(proxyAddress) {
    const implSlot = '0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc';
    const value = await client.getStorageAt({
        address: proxyAddress,
        slot: implSlot
    });
    
    const implAddress = '0x' + value.slice(26);
    console.log('Implementation:', implAddress);
    return implAddress;
}
```
{% endtab %}
{% endtabs %}
