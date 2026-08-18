---
description: >-
  Example code for the eth_blockNumber JSON RPC method. Сomplete guide on how to
  use eth_blockNumber JSON RPC in GetBlock Web3 documentation.
---

# eth\_blockNumber - Ethereum

This method returns the number of the most recent block, hex-encoded. On Ethereum mainnet this advances every \~12 seconds (one slot). Polling this method is the most common way to detect chain progress from off-chain services.

## Parameters

This method takes no parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
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
    method: 'eth_blockNumber',
    params: [],
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
        'method': 'eth_blockNumber',
        'params': [],
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
            "method": "eth_blockNumber",
            "params": [],
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
    "result": "0x1720340"
}
```

## Response Parameters

| Parameter | Type   | Description                                       |
| --------- | ------ | ------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                 |
| `id`      | string | Request identifier matching the request           |
| `result`  | string | Hex-encoded block number of the current chain tip |

## Use Cases

* **Chain Progress Monitoring**: Poll to detect that new blocks are being produced — the primary liveness signal
* **Log Backfill Boundary**: Anchor the upper bound of a range for backfilling historical event logs
* **Reorg-Safety Windows**: Wait until `blockNumber - N` before treating a transaction as finalized (typically N=32 for finalized, N=64 for defense-in-depth)
* **Confirmation Counting**: Compute `latestBlock - txBlock` to display confirmation counts in a wallet UI

## Error Handling

| Error Code | Message        | Description                                      |
| ---------- | -------------- | ------------------------------------------------ |
| -32603     | Internal error | Node failed to return the chain tip block number |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const blockNumber = await provider.getBlockNumber();
console.log('Latest block:', blockNumber);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const blockNumber = await client.getBlockNumber();
console.log('Latest block:', blockNumber);
```
{% endcode %}
{% endtab %}
{% endtabs %}
