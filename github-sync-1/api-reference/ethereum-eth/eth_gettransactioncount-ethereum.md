---
description: >-
  Example code for the eth_getTransactionCount JSON RPC method. Сomplete guide
  on how to use eth_getTransactionCount JSON RPC in GetBlock Web3 documentation.
---

# eth\_getTransactionCount - Ethereum

This method returns the number of transactions sent from an address, hex-encoded. This is the address's nonce — the counter used to order transactions from the same sender. Wallets read this before signing to set the correct nonce; nonce-management bugs are a common source of stuck or replaced transactions.

## Parameters

| Parameter        | Type   | Required | Description                                                                  |
| ---------------- | ------ | -------- | ---------------------------------------------------------------------------- |
| `address`        | string | Yes      | 20-byte address to query (hex-encoded with `0x` prefix)                      |
| `blockParameter` | string | Yes      | Block number in hex, or `latest`, `earliest`, `pending`, `finalized`, `safe` |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionCount",
    "params": [
        "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "latest"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getTransactionCount',
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
{% endtab %}

{% tab title="Request" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getTransactionCount',
        'params': [
        "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "latest"
    ],
        'id': 'getblock.io'
    }
)

print(response.json())
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
            "method": "eth_getTransactionCount",
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
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x462"
}
```

## Response Parameters

| Parameter | Type   | Description                                                                  |
| --------- | ------ | ---------------------------------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                                            |
| `id`      | string | Request identifier matching the request                                      |
| `result`  | string | Hex-encoded transaction count (nonce) for the address at the requested block |

## Use Cases

* **Nonce Management**: Set the `nonce` field when signing a transaction — typically use `pending` to include queued but unmined transactions
* **Account Activity Snapshots**: Show total sent-transaction count for an address in an explorer or wallet UI
* **Stuck-Transaction Recovery**: Diagnose whether a transaction is truly stuck by comparing on-chain nonce to expected nonce
* **Batch Transaction Sequencing**: Reliably enqueue N transactions with sequential nonces from a hot wallet

## Error Handling

| Error Code | Message        | Description                                                 |
| ---------- | -------------- | ----------------------------------------------------------- |
| -32602     | Invalid params | Address is malformed, or block parameter is invalid         |
| -32603     | Internal error | Node failed to look up account state at the requested block |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const nonce = await provider.getTransactionCount('0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', 'latest');
console.log('Nonce:', nonce);

// Use 'pending' when queueing a new transaction to account for unmined queued txs:
const pendingNonce = await provider.getTransactionCount('0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', 'pending');
console.log('Next nonce to use:', pendingNonce);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const nonce = await client.getTransactionCount({ address: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045' });
console.log('Nonce:', nonce);

const pendingNonce = await client.getTransactionCount({
    address: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045',
    blockTag: 'pending',
});
console.log('Next nonce to use:', pendingNonce);
```
{% endtab %}
{% endtabs %}
