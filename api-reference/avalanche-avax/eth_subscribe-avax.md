---
description: >-
  Example code for the eth_subscribe JSON-RPC method. Complete guide on how to
  use eth_subscribe JSON-RPC in GetBlock Web3 documentation.
---

# eth\_subscribe - AVAX

Creates a real-time subscription over a WebSocket connection on the C-Chain. Available subscription types: `newHeads` (new block headers), `logs` (matching contract events), `newPendingTransactions` (mempool), and `newAcceptedTransactions` (Avalanche-specific — fires when transactions are accepted by consensus).

{% hint style="warning" %}
Only available over WebSocket — use `wss://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/ws`. WebSocket is **not** available on P-Chain or X-Chain.
{% endhint %}

## Parameters

| Parameter        | Type   | Required | Description                                                                            |
| ---------------- | ------ | -------- | -------------------------------------------------------------------------------------- |
| subscriptionType | string | Yes      | One of `"newHeads"`, `"logs"`, `"newPendingTransactions"`, `"newAcceptedTransactions"` |
| filterObject     | object | No       | For `"logs"`: filter with `address` and/or `topics`                                    |

## Request Example

{% tabs %}
{% tab title="Axios" %}
{% code overflow="wrap" %}
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
{% endcode %}
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

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x9cef478923ff08bf67fde6c64013158d"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                                                            |
| ------ | ------ | ------------------------------------------------------------------------------------------------------ |
| result | string | Subscription ID, used to match incoming `eth_subscription` notifications and to call `eth_unsubscribe` |

## Use Cases

* Real-time wallet activity notifications
* Live DEX trade feeds for trading UIs
* Streaming indexers reacting to new blocks instantly
* Mempool surveillance (`newPendingTransactions`) or consensus-accepted feeds (`newAcceptedTransactions`)

## Error Handling

| Status Code | Error Message     | Cause                                                                    |
| ----------- | ----------------- | ------------------------------------------------------------------------ |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`                                      |
| -32602      | Invalid params    | Request parameters are missing or malformed                              |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                        |
| -32600      | Invalid Request   | `eth_subscribe` was called over HTTP; use a WebSocket connection instead |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js (WebSocket)" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.WebSocketProvider('wss://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/ws');

provider.on('block', (blockNumber) => {
    console.log('New block:', blockNumber);
});
```
{% endcode %}
{% endtab %}

{% tab title="Viem (WebSocket)" %}
{% code overflow="wrap" %}
```javascript
import { createPublicClient, webSocket } from 'viem';
import { avalanche } from 'viem/chains';

const client = createPublicClient({
    chain: avalanche,
    transport: webSocket('wss://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/ws')
});

const unwatch = client.watchBlockNumber({
    onBlockNumber: (blockNumber) => console.log('New block:', blockNumber)
});
```
{% endcode %}
{% endtab %}
{% endtabs %}
