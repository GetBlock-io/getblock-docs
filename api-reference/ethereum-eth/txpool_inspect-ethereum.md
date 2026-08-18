---
description: >-
  Example code for the txpool_inspect JSON RPC method. Complete guide on how to
  use txpool_inspect JSON RPC in GetBlock Web3 documentation.
---

# txpool\_inspect - Ethereum

This method returns a compact, human-readable summary of the pending and queued transactions in the node's transaction pool. It uses the same sender-and-nonce nesting as [txpool\_content](txpool_content-ethereum.md), but reduces each transaction to a single summary string instead of a full object, which makes the response dramatically smaller.

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
    "method": "txpool_inspect",
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
    method: 'txpool_inspect',
    params: [],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

for (const [sender, byNonce] of Object.entries(response.data.result.pending)) {
    for (const [nonce, summary] of Object.entries(byNonce)) {
        console.log(`${sender} #${nonce} -> ${summary}`);
    }
}
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
        'method': 'txpool_inspect',
        'params': [],
        'id': 'getblock.io'
    }
)

pending = response.json()['result']['pending']
for sender, by_nonce in pending.items():
    for nonce, summary in by_nonce.items():
        print(f"{sender} #{nonce} -> {summary}")
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
            "method": "txpool_inspect",
            "params": [],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Pending senders: {}", response["result"]["pending"].as_object().unwrap().len());
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
        "pending": {
            "0x000025e01DB606436e2A658C765CcB78442b1c69": {
                "0": "0xdAC17F958D2ee523a2206206994597C13D831ec7: 0 wei + 23000 gas × 10080000 wei"
            },
            "0x00003968a75E1D8A6d13569F5B6ad7Ba5e710000": {
                "1": "0xdE992BAdD92aDaf9146255E9Bde6a70770973664: 1038330323400 wei + 21000 gas × 66132588 wei"
            }
        },
        "queued": {
            "0x000000084002Fb85b89bEc70045B9Ec77b3d4C38": {
                "74": "0xda3bD1fE1973470312db04551B65f401Bc8a92fD: 0 wei + 730875 gas × 27000000000 wei"
            }
        }
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                                 |
| --------- | ------ | --------------------------------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                                           |
| `id`      | string | Request identifier matching the request                                     |
| `pending` | object | Transactions eligible for inclusion, keyed by sender address, then by nonce |
| `queued`  | object | Transactions not yet processable, with the same nesting                     |

Each leaf value is a string of the form `<to>: <value> wei + <gas> gas × <gasPrice> wei`. Unlike `txpool_content`, all numbers here are decimal rather than hex-encoded.

| Segment    | Description                  |
| ---------- | ---------------------------- |
| `to`       | Recipient address            |
| `value`    | ETH transferred, in wei      |
| `gas`      | Gas limit of the transaction |
| `gasPrice` | Gas price offered, in wei    |

## Use Cases

* **Lightweight Mempool Surveys**: Read the shape of the pool without transferring tens of megabytes
* **Gas Price Monitoring**: Spot senders bidding unusually high gas prices for inclusion
* **Queue Diagnostics**: Identify nonce gaps that strand transactions in the queued set
* **Dashboards and Alerting**: Feed cheap, frequent mempool snapshots into monitoring

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

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const inspect = await provider.send('txpool_inspect', []);
console.log(Object.keys(inspect.pending).length, 'senders with pending transactions');
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

const inspect = await client.request({ method: 'txpool_inspect' });
console.log(Object.keys(inspect.pending).length, 'senders with pending transactions');
```
{% endcode %}
{% endtab %}
{% endtabs %}
