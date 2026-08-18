---
description: >-
  Example code for the eth_subscribe JSON RPC method. Сomplete guide on how to
  use eth_subscribe JSON RPC in GetBlock Web3 documentation.
---

# eth\_subscribe - Ethereum

This method creates a real-time subscription over a WebSocket connection to receive push notifications for chain events.  Returns a subscription ID; the node then pushes notifications tagged with that ID.

{% hint style="warning" %}
Requires a WSS endpoint (not HTTP). Supported subscription types: `newHeads` (new block headers), `logs` (matching event logs), `newPendingTransactions` (mempool transaction hashes), and `syncing` (node sync state changes).
{% endhint %}

## Parameters

| Parameter          | Type   | Required | Description                                                                                              |
| ------------------ | ------ | -------- | -------------------------------------------------------------------------------------------------------- |
| `subscriptionType` | string | Yes      | One of `newHeads`, `logs`, `newPendingTransactions`, `syncing`                                           |
| `filter`           | object | No       | Filter criteria — only applies to `logs` (same shape as `eth_newFilter` filter object minus block range) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_subscribe",
    "params": [
        "logs",
        {
            "address": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
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
    method: 'eth_subscribe',
    params: [
        "logs",
        {
            "address": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
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
        'method': 'eth_subscribe',
        'params': [
        "logs",
        {
            "address": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
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
            "method": "eth_subscribe",
            "params": [
        "logs",
        {
            "address": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
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
    "result": "0x9cef478923ff08bf67fde6c64013158d"
}
```

## Response Parameters

| Parameter | Type   | Description                                                                                     |
| --------- | ------ | ----------------------------------------------------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                                                               |
| `id`      | string | Request identifier matching the request                                                         |
| `result`  | string | Hex-encoded subscription ID — matches the `subscription` field on subsequent push notifications |

## Use Cases

* **Real-Time Log Streaming**: Subscribe to `logs` for a contract to receive events with lower latency than polling
* **Live Block Notifications**: Subscribe to `newHeads` to drive per-block backend workflows without polling `eth_blockNumber`
* **Mempool Monitoring**: Subscribe to `newPendingTransactions` for MEV, front-run detection, or wallet activity feeds
* **Sync State Alerts**: Subscribe to `syncing` on a self-hosted node to alert when it falls behind

## Error Handling

| Error Code | Message        | Description                                          |
| ---------- | -------------- | ---------------------------------------------------- |
| -32602     | Invalid params | Subscription type unsupported or filter is malformed |
| -32603     | Internal error | Node failed to create the subscription               |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

// WebSocket endpoint required for subscriptions
const provider = new ethers.WebSocketProvider('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

// High-level: use built-in event subscriptions (Ethers manages the subscription)
provider.on('block', (blockNumber) => {
    console.log('New block:', blockNumber);
});

const filter = {
    address: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
    topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'],
};
provider.on(filter, (log) => {
    console.log('Transfer log:', log);
});
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, webSocket } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: webSocket('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

// Subscribe to new block headers
const unwatch = client.watchBlocks({
    onBlock: (block) => console.log('New block:', block.number),
});

// Subscribe to matching logs
const unwatchLogs = client.watchEvent({
    address: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
    onLogs: (logs) => logs.forEach(l => console.log('Transfer log:', l)),
});
```
{% endcode %}
{% endtab %}
{% endtabs %}
