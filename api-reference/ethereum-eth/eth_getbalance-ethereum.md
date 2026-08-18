---
description: >-
  Example code for the eth_getBalance JSON RPC method. Сomplete guide on how to
  use eth_getBalance JSON RPC in GetBlock Web3 documentation.
---

# eth\_getBalance - Ethereum

This method returns the ETH balance of a specific address at a given block, in wei, hex-encoded. It's the fundamental read for any wallet UI, block explorer, or accounting system tracking native-token holdings.

## Parameters

| Parameter        | Type   | Required | Description                                                                  |
| ---------------- | ------ | -------- | ---------------------------------------------------------------------------- |
| `address`        | string | Yes      | 20-byte address to query (hex-encoded with `0x` prefix)                      |
| `blockParameter` | string | Yes      | Block number in hex, or `latest`, `earliest`, `pending`, `finalized`, `safe` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": [
        "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "latest"
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getBalance',
    params: [
        "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "latest"
    ],
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getBalance',
        'params': [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "latest"
        ],
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getBalance",
            "params": [
                "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
                "latest"
            ],
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
    "result": "0x2c68af0bb140000"
}
```

## Response Parameters

| Parameter | Type   | Description                                             |
| --------- | ------ | ------------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                       |
| `id`      | string | Request identifier matching the request                 |
| `result`  | string | Hex-encoded balance in wei (divide by 10^18 to get ETH) |

## Use Cases

* **Wallet Balance Display**: Show a user's native ETH balance in a wallet or dApp UI
* **Portfolio Tracking**: Aggregate ETH balances across many addresses for portfolio analytics
* **Solvency Checks**: Verify an address has enough ETH to cover a gas budget before submitting a transaction
* **Historical Balance Snapshots**: Query balances at specific historical blocks for audits or airdrops

## Error Handling

| Error Code | Message        | Description                                                                |
| ---------- | -------------- | -------------------------------------------------------------------------- |
| -32602     | Invalid params | Address is not a valid 20-byte hex string, or block parameter is malformed |
| -32603     | Internal error | Node failed to look up state at the requested block                        |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const balanceWei = await provider.getBalance('0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045');
console.log('Balance (wei):', balanceWei.toString());
console.log('Balance (ETH):', ethers.formatEther(balanceWei));
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" overflow="wrap" %}
```javascript
import { createPublicClient, http, formatEther } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const balanceWei = await client.getBalance({ address: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045' });
console.log('Balance (wei):', balanceWei);
console.log('Balance (ETH):', formatEther(balanceWei));
```
{% endcode %}
{% endtab %}
{% endtabs %}
