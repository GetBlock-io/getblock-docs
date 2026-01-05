---
description: >-
  Example code for the eth_unsubscribe JSON-RPC method. Complete guide on how to
  use eth_unsubscribe JSON-RPC in GetBlock Web3 documentation.
---

# eth\_unsubscribe - Mantle

This method cancels an existing subscription. It requires a WebSocket connection.

### Parameters

| Parameter      | Type   | Description                                    |
| -------------- | ------ | ---------------------------------------------- |
| subscriptionId | string | The subscription ID returned by eth\_subscribe |

{% hint style="info" %}
eth\_unsubscribe requires a WebSocket connection.
{% endhint %}

### Request (WebSocket)

{% tabs %}
{% tab title="JavaScript (WebSocket)" %}
{% code title="ws-example.js" %}
```javascript
// WebSocket connection required
const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

ws.onopen = () => {
    ws.send(JSON.stringify({
        "jsonrpc": "2.0",
        "method": "eth_unsubscribe",
        "params": ["0x9ce59a13059e417087c02d3236a0b1cc"],
        "id": "getblock.io"
    }));
};

ws.onmessage = (event) => {
    console.log('Received:', JSON.parse(event.data));
};
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (axios - note)" %}
{% code title="axios-note.js" %}
```javascript
import axios from 'axios';

// Note: eth_unsubscribe requires WebSocket
const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_unsubscribe",
    "params": ["0x9ce59a13059e417087c02d3236a0b1cc"],
    "id": "getblock.io"
});

console.log('WebSocket message:', data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="ws-example.py" %}
```python
import websocket
import json

def on_message(ws, message):
    print(f"Received: {message}")

def on_open(ws):
    ws.send(json.dumps({
        "jsonrpc": "2.0",
        "method": "eth_unsubscribe",
        "params": ["0x9ce59a13059e417087c02d3236a0b1cc"],
        "id": "getblock.io"
    }))

ws = websocket.WebSocketApp(
    "wss://go.getblock.io/<ACCESS-TOKEN>/",
    on_message=on_message,
    on_open=on_open
)
ws.run_forever()
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="rust-example.rs" %}
```rust
// Note: eth_unsubscribe requires WebSocket connection
let message = r#"{
    "jsonrpc": "2.0",
    "method": "eth_unsubscribe",
    "params": ["0x9ce59a13059e417087c02d3236a0b1cc"],
    "id": "getblock.io"
}"#;
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": true
}
```
{% endcode %}

### Response Parameters

| Field  | Type    | Description                                     |
| ------ | ------- | ----------------------------------------------- |
| result | boolean | True if subscription was cancelled successfully |

### Use Case

The `eth_unsubscribe` method is essential for:

* Cleaning up subscriptions
* Resource management
* Stopping event streams
* Connection lifecycle management

### Error Handling

| Status Code | Error Message          | Cause                           |
| ----------- | ---------------------- | ------------------------------- |
| 403         | Forbidden              | Missing or invalid ACCESS-TOKEN |
| -32000      | Subscription not found | Invalid subscription ID         |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-unsubscribe.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.WebSocketProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');

// Unsubscribe by removing listener
provider.off('block');
// Or close the connection
await provider.destroy();
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-unsubscribe.js" %}
```javascript
import { createPublicClient, webSocket } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: webSocket('wss://go.getblock.io/<ACCESS-TOKEN>/')
});

const unwatch = client.watchBlocks({
    onBlock: (block) => console.log(block)
});

// Unsubscribe
unwatch();
```
{% endcode %}
{% endtab %}
{% endtabs %}
