---
description: >-
  Example code for the eth_getBlockTransactionCountByNumber JSON RPC method.
  Сomplete guide on how to use eth_getBlockTransactionCountByNumber JSON RPC in
  GetBlock Web3 documentation.
---

# eth\_getBlockTransactionCountByNumber - Ethereum

This method returns the number of transactions in a block, identified by block number or tag, hex-encoded. Same output as `eth_getBlockTransactionCountByHash`, but with the more convenient block-number-based lookup.

## Parameters

| Parameter        | Type   | Required | Description                                                                  |
| ---------------- | ------ | -------- | ---------------------------------------------------------------------------- |
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
    "method": "eth_getBlockTransactionCountByNumber",
    "params": [
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
    method: 'eth_getBlockTransactionCountByNumber',
    params: [
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
        'method': 'eth_getBlockTransactionCountByNumber',
        'params': [
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
            "method": "eth_getBlockTransactionCountByNumber",
            "params": [
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
    "result": "0xde"
}
```

## Response Parameters

| Parameter | Type   | Description                                |
| --------- | ------ | ------------------------------------------ |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")          |
| `id`      | string | Request identifier matching the request    |
| `result`  | string | Hex-encoded transaction count in the block |

## Use Cases

* **Live Chain Activity Monitoring**: Poll `latest` to detect throughput fluctuations without loading full blocks
* **Historical Activity Charts**: Iterate through block numbers to build per-block transaction-count charts
* **Iteration Bounds**: Determine the loop bound for iterating transactions by `(blockNumber, index)`

## Error Handling

| Error Code | Message        | Description                      |
| ---------- | -------------- | -------------------------------- |
| -32602     | Invalid params | Block parameter is malformed     |
| -32603     | Internal error | Node failed to look up the block |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const countHex = await provider.send('eth_getBlockTransactionCountByNumber', ['latest']);
console.log('Transaction count:', parseInt(countHex, 16));
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

const count = await client.getBlockTransactionCount({ blockTag: 'latest' });
console.log('Transaction count:', count);
```
{% endcode %}
{% endtab %}
{% endtabs %}
