---
description: >-
  Example code for the eth_subscribe JSON-RPC method. Complete guide on how to
  use eth_subscribe JSON-RPC in GetBlock Web3 documentation.
---

# eth\_subscribe - ARC

This method creates a real-time subscription over a WebSocket connection. Available subscription types are `newHeads` (new block headers), `logs` (matching contract events), and `newPendingTransactions` (pending transactions).&#x20;

{% hint style="info" %}
This method is only available over WebSocket transport — use `wss://go.getblock.io/<ACCESS-TOKEN>/`. With Arc's \~2 second block times, WebSocket subscriptions are the most efficient way to drive real-time payment UIs.
{% endhint %}

## Parameters

| Parameter        | Type   | Required | Description                                               |
| ---------------- | ------ | -------- | --------------------------------------------------------- |
| subscriptionType | string | Yes      | One of `"newHeads"`, `"logs"`, `"newPendingTransactions"` |
| filterObject     | object | No       | For `"logs"`: filter with `address` and/or `topics`       |

## Request Example

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

* Real-time merchant payment confirmation notifications
* Live FX engine trade feeds for trading UIs
* Streaming indexers that react to new blocks instantly
* Compliance systems monitoring high-value transfers in real time

## Error Handling

| Status Code | Error Message     | Cause                                                                    |
| ----------- | ----------------- | ------------------------------------------------------------------------ |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                      |
| -32602      | Invalid params    | Request parameters are missing or malformed                              |
| -32601      | Method not found  | The method is not supported by this node                                 |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                        |
| -32600      | Invalid Request   | `eth_subscribe` was called over HTTP; use a WebSocket connection instead |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js (WebSocket)" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.WebSocketProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');

provider.on('block', (blockNumber) => {
    console.log('New block:', blockNumber);
});
```
{% endtab %}

{% tab title="Viem (WebSocket)" %}
```javascript
import { createPublicClient, webSocket, defineChain } from 'viem';

const arcTestnet = defineChain({
    id: 5042002,
    name: 'Arc Testnet',
    nativeCurrency: { name: 'USD Coin', symbol: 'USDC', decimals: 6 },
    rpcUrls: { default: { http: ['https://go.getblock.io/<ACCESS-TOKEN>/'], webSocket: ['wss://go.getblock.io/<ACCESS-TOKEN>/'] } }
});

const client = createPublicClient({
    chain: arcTestnet,
    transport: webSocket('wss://go.getblock.io/<ACCESS-TOKEN>/')
});

const unwatch = client.watchBlockNumber({
    onBlockNumber: (blockNumber) => console.log('New block:', blockNumber)
});
```
{% endtab %}
{% endtabs %}
