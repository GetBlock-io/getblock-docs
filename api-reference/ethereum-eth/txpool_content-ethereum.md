---
description: >-
  Example code for the txpool_content JSON RPC method. Complete guide on how to
  use txpool_content JSON RPC in GetBlock Web3 documentation.
---

# txpool\_content - Ethereum

This method returns every pending and queued transaction currently held in the node's transaction pool, grouped first by sender address and then by nonce. Pending transactions are eligible for inclusion in the next block; queued transactions are held back, usually because of a gap in the sender's nonce sequence.

{% hint style="warning" %}
The Ethereum mainnet mempool routinely holds tens of thousands of transactions, so an unfiltered `txpool_content` response is very large — a single call can return well over 80 MB and take many seconds to transfer. Prefer [txpool\_status](txpool_status-ethereum.md) when you only need counts, [txpool\_inspect](txpool_inspect-ethereum.md) for a compact text summary, or [txpool\_contentFrom](txpool_contentfrom-ethereum.md) to scope the result to one sender.
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
    "method": "txpool_content",
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
    method: 'txpool_content',
    params: [],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const { pending, queued } = response.data.result;
console.log('Pending senders:', Object.keys(pending).length);
console.log('Queued senders:', Object.keys(queued).length);
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
        'method': 'txpool_content',
        'params': [],
        'id': 'getblock.io'
    }
)

result = response.json()['result']
print(f"Pending senders: {len(result['pending'])}")
print(f"Queued senders: {len(result['queued'])}")
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
            "method": "txpool_content",
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

Truncated to one pending and one queued transaction; a real response contains many thousands of entries.

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "pending": {
            "0x00003968a75E1D8A6d13569F5B6ad7Ba5e710000": {
                "1": {
                    "blockHash": null,
                    "blockNumber": null,
                    "blockTimestamp": null,
                    "from": "0x00003968a75e1d8a6d13569f5b6ad7ba5e710000",
                    "gas": "0x5208",
                    "gasPrice": "0x3f11a6c",
                    "maxFeePerGas": "0x3f11a6c",
                    "maxPriorityFeePerGas": "0x2673fa",
                    "hash": "0xd8b669c68465d8047e8b691fcf615b25b7c1b45565f2c3280b4d5829d8cfc384",
                    "input": "0x",
                    "nonce": "0x1",
                    "to": "0xde992badd92adaf9146255e9bde6a70770973664",
                    "transactionIndex": null,
                    "value": "0xf1c14f61c8",
                    "type": "0x2",
                    "accessList": [],
                    "chainId": "0x1",
                    "v": "0x0",
                    "r": "0xf584ba938aa9a4d289e8b48990772c8518d0a2ca19b01c6b56f593217e2a10bb",
                    "s": "0x6067dfd6b3f80f0eb3f30ec7d693d14b61223d4d3ef1669b7c8300be5a0cebe4",
                    "yParity": "0x0"
                }
            }
        },
        "queued": {
            "0x00A6006de8cb5A89237E61365DfEC2B8ae91cDb3": {
                "34": {
                    "blockHash": null,
                    "blockNumber": null,
                    "blockTimestamp": null,
                    "from": "0x00a6006de8cb5a89237e61365dfec2b8ae91cdb3",
                    "gas": "0xc350",
                    "gasPrice": "0x3b9aca00",
                    "hash": "0xeb85607edd7594d20dc4a7f8a942c5004274b32a42d966efdf8926bf002e7f3d",
                    "input": "0xa0712d680000000000000000000000000000000000000000000000000000000000000001",
                    "nonce": "0x22",
                    "to": "0xe622d2ae9fa0ee2e2fb21506e21b78bdb77db6e7",
                    "transactionIndex": null,
                    "value": "0x0",
                    "type": "0x0",
                    "chainId": "0x1",
                    "v": "0x25",
                    "r": "0x6f59eca36264fe87bc30b6243740fbaca3b99be6fcec855c6f6c50500978f30a",
                    "s": "0xb8b5616233b4aea718933a39b7dc24867de204db0fc6abff141d3e216964bde"
                }
            }
        }
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                                               |
| --------- | ------ | ----------------------------------------------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                                                         |
| `id`      | string | Request identifier matching the request                                                   |
| `pending` | object | Transactions eligible for inclusion, keyed by sender address, then by decimal nonce       |
| `queued`  | object | Transactions not yet processable (typically a nonce gap), with the same two-level nesting |

Each transaction object carries the standard Ethereum transaction fields:

| Field                                                               | Type   | Description                                                    |
| ------------------------------------------------------------------- | ------ | -------------------------------------------------------------- |
| `blockHash` / `blockNumber` / `blockTimestamp` / `transactionIndex` | null   | Always `null` while the transaction is still in the pool       |
| `from` / `to`                                                       | string | Sender and recipient. `to` is `null` for contract creation     |
| `gas` / `gasPrice`                                                  | string | Gas limit and effective gas price, hex-encoded                 |
| `maxFeePerGas` / `maxPriorityFeePerGas`                             | string | EIP-1559 fee fields, present on `type` `0x2` transactions      |
| `nonce`                                                             | string | Sender nonce, hex-encoded                                      |
| `value`                                                             | string | ETH transferred, in wei, hex-encoded                           |
| `input`                                                             | string | Encoded call data                                              |
| `type`                                                              | string | Transaction type: `0x0` legacy, `0x1` EIP-2930, `0x2` EIP-1559 |
| `accessList`                                                        | array  | EIP-2930 access list, present on typed transactions            |
| `chainId`                                                           | string | `0x1` for Ethereum mainnet                                     |
| `v` / `r` / `s` / `yParity`                                         | string | Signature components                                           |

## Use Cases

* **Mempool Analytics**: Measure pending demand and fee distribution across the whole pool
* **MEV and Arbitrage**: React to unconfirmed transactions before they are included in a block
* **Stuck Transaction Diagnosis**: Find nonce gaps that leave an account's transactions in the queued set
* **Fee Estimation**: Derive a competitive gas price from what is actually pending rather than from historical blocks

## Error Handling

| Error Code | Message          | Description                                                  |
| ---------- | ---------------- | ------------------------------------------------------------ |
| -32601     | Method not found | The `txpool` module is not enabled on this endpoint          |
| -32603     | Internal error   | Node failed to serialise the pool, often because of its size |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const content = await provider.send('txpool_content', []);
console.log('Pending senders:', Object.keys(content.pending).length);
console.log('Queued senders:', Object.keys(content.queued).length);
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

const content = await client.request({ method: 'txpool_content' });
console.log('Pending senders:', Object.keys(content.pending).length);
```
{% endcode %}
{% endtab %}
{% endtabs %}
