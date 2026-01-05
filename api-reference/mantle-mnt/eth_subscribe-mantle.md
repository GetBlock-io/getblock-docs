---
description: >-
  Example code for the eth_subscribe JSON-RPC method. Complete guide on how to
  use eth_subscribe JSON-RPC in GetBlock Web3 documentation.
---

# eth\_subscribe - Mantle

This method creates a subscription for specific events on the blockchain.&#x20;

{% hint style="warning" %}
It requires a WebSocket connection. When an event occurs, the server pushes a notification to the client.
{% endhint %}

### Parameters

| Parameter        | Type   | Description                                                         |
| ---------------- | ------ | ------------------------------------------------------------------- |
| subscriptionType | string | Type of subscription ("newHeads", "logs", "newPendingTransactions") |
| filterObject     | object | (optional) Filter options for logs subscription                     |

### Request (WebSocket)

{% tabs %}
{% tab title="Axios" %}
```js
import axios from 'axios';

// Note: eth_subscribe requires WebSocket, but here's the message format
const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_subscribe",
    "params": ["newHeads"],
    "id": "getblock.io"
});

console.log('WebSocket message:', data);
```
{% endtab %}

{% tab title="JavaScript (WebSocket & message format)" %}
```javascript
// WebSocket connection required
const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

ws.onopen = () => {
    ws.send(JSON.stringify({
        "jsonrpc": "2.0",
        "method": "eth_subscribe",
        "params": ["newHeads"],
        "id": "getblock.io"
    }));
};

ws.onmessage = (event) => {
    console.log('Received:', JSON.parse(event.data));
};
```
{% endtab %}

{% tab title="Python" %}
```python
import websocket
import json

def on_message(ws, message):
    print(f"Received: {message}")

def on_open(ws):
    ws.send(json.dumps({
        "jsonrpc": "2.0",
        "method": "eth_subscribe",
        "params": ["newHeads"],
        "id": "getblock.io"
    }))

ws = websocket.WebSocketApp(
    "wss://go.getblock.io/<ACCESS-TOKEN>/",
    on_message=on_message,
    on_open=on_open
)
ws.run_forever()
```
{% endtab %}

{% tab title="Rust" %}
```rust
// Note: eth_subscribe requires WebSocket connection
// Example message format for WebSocket client
let message = r#"{
    "jsonrpc": "2.0",
    "method": "eth_subscribe",
    "params": ["newHeads"],
    "id": "getblock.io"
}"#;
```
{% endtab %}
{% endtabs %}

### Subscription Types

| Type                   | Description                                  |
| ---------------------- | -------------------------------------------- |
| newHeads               | Fires when new block is added to chain       |
| logs                   | Fires when logs matching filter are included |
| newPendingTransactions | Fires when new pending tx is added to pool   |

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x9ce59a13059e417087c02d3236a0b1cc"
}
```

### Notification Example (newHeads)

```json
{
    "jsonrpc": "2.0",
    "method": "eth_subscription",
    "params": {
        "subscription": "0x9ce59a13059e417087c02d3236a0b1cc",
        "result": {
            "number": "0x3f6778",
            "hash": "0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e",
            "parentHash": "0x...",
            "timestamp": "0x..."
        }
    }
}
```

### Response Parameters

| Field  | Type   | Description                                   |
| ------ | ------ | --------------------------------------------- |
| result | string | Subscription ID for managing the subscription |

### Use Case

The `eth_subscribe` method is essential for:

* Real-time block monitoring
* Event streaming
* Live transaction tracking
* DApp real-time updates
* Notification systems

### Error Handling

| Status Code | Error Message              | Cause                           |
| ----------- | -------------------------- | ------------------------------- |
| 403         | Forbidden                  | Missing or invalid ACCESS-TOKEN |
| -32000      | Subscription not supported | HTTP used instead of WebSocket  |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.WebSocketProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');

provider.on('block', (blockNumber) => {
    console.log('New block:', blockNumber);
});
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, webSocket } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: webSocket('wss://go.getblock.io/<ACCESS-TOKEN>/')
});

const unwatch = client.watchBlocks({
    onBlock: (block) => console.log('New block:', block.number)
});
```
{% endtab %}
{% endtabs %}
