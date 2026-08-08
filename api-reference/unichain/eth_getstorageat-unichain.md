---
description: >-
  Example code for the eth_getStorageAt JSON-RPC method. Complete guide to using
  the eth_getStorageAt JSON-RPC method in the GetBlock.io Web3 documentation.
---

# eth\_getStorageAt - Unichain

This method returns the value stored at a given storage slot of a contract at a given block. It reads raw contract storage directly.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| address        | string | Yes      | 20-byte contract address                                |
| position       | string | Yes      | Hex-encoded storage slot index                          |
| blockParameter | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
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
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getStorageAt',
    params: ["0x4200000000000000000000000000000000000006", "0x0", "latest"],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getStorageAt',
        'params': ["0x4200000000000000000000000000000000000006", "0x0", "latest"],
        'id': 'getblock.io'
    }
)

print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
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
            "params": ["0x4200000000000000000000000000000000000006", "0x0", "latest"],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;

    println!("Result: {}", response["result"]);
    Ok(())
}
```
{% endcode %}
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

| Parameter | Type   | Description                                   |
| --------- | ------ | --------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")             |
| id        | string | Request identifier matching the request       |
| result    | string | 32-byte hex-encoded value at the storage slot |

## Use Cases

* **Raw Storage Reads**: Read state that a contract does not expose through a getter
* **Proxy Slots**: Read the implementation address from an EIP-1967 proxy slot
* **Debugging**: Inspect a contract's raw storage layout
* **Verification**: Confirm a storage value against expected state

## Error Handling

| Error Code | Message        | Description                                                                |
| ---------- | -------------- | -------------------------------------------------------------------------- |
| -32602     | Invalid params | The address is not a valid 20-byte hex string, or the block tag is invalid |
| -32603     | Internal error | The node failed to read the requested state                                |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const value = await provider.getStorage('0x4200000000000000000000000000000000000006', 0);
console.log(value);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { unichain } from 'viem/chains';

const client = createPublicClient({ chain: unichain, transport: http('https://go.getblock.io/<ACCESS-TOKEN>/') });

const value = await client.getStorageAt({ address: '0x4200000000000000000000000000000000000006', slot: '0x0' });
console.log(value);
```
{% endcode %}
{% endtab %}
{% endtabs %}
