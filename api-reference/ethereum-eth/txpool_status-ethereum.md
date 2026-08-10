---
description: >-
  Example code for the txpool_status JSON RPC method. Complete guide on how to
  use txpool_status JSON RPC in GetBlock Web3 documentation.
---

# txpool\_status - Ethereum

This method returns the number of pending and queued transactions currently held in the node's transaction pool. It is the cheapest of the `txpool_*` methods: the response is two integers regardless of how busy the mempool is.

## Parameters

This method takes no parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "txpool_status",
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

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'txpool_status',
    params: [],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const result = response.data.result;
console.log('Pending:', parseInt(result.pending, 16));
console.log('Queued:', parseInt(result.queued, 16));
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
        'method': 'txpool_status',
        'params': [],
        'id': 'getblock.io'
    }
)

result = response.json()['result']
print(f"Pending: {int(result['pending'], 16)}")
print(f"Queued: {int(result['queued'], 16)}")
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
            "method": "txpool_status",
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
    "result": {
        "pending": "0x1739e",
        "queued": "0x36f3"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                                   |
| --------- | ------ | ----------------------------------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                                             |
| `id`      | string | Request identifier matching the request                                       |
| `pending` | string | Hex-encoded count of transactions eligible for inclusion (`0x1739e` = 95,134) |
| `queued`  | string | Hex-encoded count of transactions not yet processable (`0x36f3` = 14,067)     |

## Use Cases

* **Congestion Monitoring**: Track mempool depth over time to gauge network load
* **Confirmation Estimates**: Combine pending depth with block gas limits to estimate wait times
* **Node Health Checks**: Confirm an endpoint is peered and receiving transactions
* **Cheap Polling**: Detect mempool changes before paying for a full [txpool\_content](txpool_content-ethereum.md) call

## Error Handling

| Error Code | Message          | Description                                         |
| ---------- | ---------------- | --------------------------------------------------- |
| -32601     | Method not found | The `txpool` module is not enabled on this endpoint |
| -32603     | Internal error   | Node failed to read the transaction pool            |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const status = await provider.send('txpool_status', []);
console.log('Pending:', parseInt(status.pending, 16));
console.log('Queued:', parseInt(status.queued, 16));
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
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/'),
});

const status = await client.request({ method: 'txpool_status' });
console.log('Pending:', parseInt(status.pending, 16));
```
{% endcode %}
{% endtab %}
{% endtabs %}
