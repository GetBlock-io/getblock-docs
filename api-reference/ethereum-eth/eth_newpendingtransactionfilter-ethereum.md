---
description: >-
  Example code for the eth_newPendingTransactionFilter JSON RPC method. Сomplete
  guide on how to use eth_newPendingTransactionFilter JSON RPC in GetBlock Web3
  documentation.
---

# eth\_newPendingTransactionFilter - Ethereum

This method creates a filter that fires on new transactions entering the node's mempool. Returns a filter ID that subsequent `eth_getFilterChanges` calls poll to receive pending transaction hashes.&#x20;

{% hint style="info" %}
Note that visibility of pending transactions is inherently node-specific — public RPC endpoints typically see only a partial mempool view.
{% endhint %}

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
    "method": "eth_newPendingTransactionFilter",
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
    method: 'eth_newPendingTransactionFilter',
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
        'method': 'eth_newPendingTransactionFilter',
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
            "method": "eth_newPendingTransactionFilter",
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
    "result": "0x1b"
}
```

## Response Parameters

| Parameter | Type   | Description                                                                                      |
| --------- | ------ | ------------------------------------------------------------------------------------------------ |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                                                                |
| `id`      | string | Request identifier matching the request                                                          |
| `result`  | string | Hex-encoded filter ID — pass to `eth_getFilterChanges` to receive new pending transaction hashes |

## Use Cases

* **Wallet Post-Submit Tracking**: Track a user's just-submitted transaction as it enters the mempool
* **Mempool Monitoring**: Feed pending transaction hashes into mempool analytics or MEV detection systems
* **Live Activity Dashboards**: Show a 'live pending' feed of transactions arriving at the node

## Error Handling

| Error Code | Message        | Description                      |
| ---------- | -------------- | -------------------------------- |
| -32603     | Internal error | Node failed to create the filter |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const filterId = await provider.send('eth_newPendingTransactionFilter', []);
console.log('Pending tx filter ID:', filterId);

setInterval(async () => {
    const hashes = await provider.send('eth_getFilterChanges', [filterId]);
    hashes.forEach(h => console.log('Pending tx:', h));
}, 2000);
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

const filter = await client.createPendingTransactionFilter();
console.log('Pending tx filter ID:', filter.id);

setInterval(async () => {
    const hashes = await client.getFilterChanges({ filter });
    hashes.forEach(h => console.log('Pending tx:', h));
}, 2000);
```
{% endcode %}
{% endtab %}
{% endtabs %}
