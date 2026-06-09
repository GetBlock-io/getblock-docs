# eth\_unsubscribe tempo

{% hint style="info" %}
**WebSocket-only method.** Available only over the WebSocket transport at `wss://go.getblock.io/<ACCESS-TOKEN>/`. Calling it over HTTP POST returns an `Invalid Request` error.
{% endhint %}

Cancels an existing subscription created with `eth_subscribe`. Like `eth_subscribe`, only available over WebSocket transport.

## Parameters

| Parameter      | Type   | Required | Description                                     |
| -------------- | ------ | -------- | ----------------------------------------------- |
| subscriptionId | string | Yes      | The subscription ID returned by `eth_subscribe` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# WebSocket-only method. Use wscat (or similar) to connect first:
wscat -c 'wss://go.getblock.io/<ACCESS-TOKEN>/'

# Then send:
{"jsonrpc": "2.0", "method": "eth_unsubscribe", "params": ["0x9cef478923ff08bf67fde6c64013158d"], "id": "getblock.io"}
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import WebSocket from 'ws';

const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

ws.on('open', () => {
    ws.send(JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_unsubscribe",
    "params": [
        "0x9cef478923ff08bf67fde6c64013158d"
    ],
    "id": "getblock.io"
}));
});

ws.on('message', (data) => {
    console.log(JSON.parse(data.toString()));
});
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import asyncio
import json
import websockets

async def main():
    async with websockets.connect('wss://go.getblock.io/<ACCESS-TOKEN>/') as ws:
        await ws.send(json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_unsubscribe",
    "params": [
        "0x9cef478923ff08bf67fde6c64013158d"
    ],
    "id": "getblock.io"
}))
        async for message in ws:
            print(json.loads(message))

asyncio.run(main())
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use tokio_tungstenite::connect_async;
use tokio_tungstenite::tungstenite::Message;
use serde_json::json;
use futures_util::{SinkExt, StreamExt};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let (mut ws_stream, _) = connect_async("wss://go.getblock.io/<ACCESS-TOKEN>/").await?;

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_unsubscribe",
        "params": [
                "0x9cef478923ff08bf67fde6c64013158d"
        ],
        "id": "getblock.io"
});
    ws_stream.send(Message::Text(payload.to_string())).await?;

    while let Some(msg) = ws_stream.next().await {
        println!("{:?}", msg?);
    }
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": true
}
```
{% endcode %}

## Response Parameters

| Field  | Type    | Description                                                           |
| ------ | ------- | --------------------------------------------------------------------- |
| result | boolean | `true` if the subscription was cancelled, `false` if it did not exist |

## Use Cases

* Stopping a live feed when the user navigates away
* Reconnecting subscribers after a connection drop
* Cleanup in WebSocket-based microservices

## Error Handling

| Status Code | Error Message     | Cause                                                                      |
| ----------- | ----------------- | -------------------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                        |
| -32602      | Invalid params    | Request parameters are missing or malformed                                |
| -32601      | Method not found  | Method does not exist or is not enabled on this node                       |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                          |
| -32600      | Invalid Request   | `eth_unsubscribe` was called over HTTP; use a WebSocket connection instead |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_unsubscribe', ["0x9cef478923ff08bf67fde6c64013158d"]);
console.log(result);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http, defineChain } from 'viem';

// Tempo is not yet in viem/chains as a built-in — define it inline.
const tempo = defineChain({
    id: 4217,
    name: 'Tempo',
    network: 'tempo',
    // Tempo has no native gas token. The `nativeCurrency` field is required by viem's
    // type system; values shown here are a no-op placeholder — actual fees are paid in TIP-20.
    nativeCurrency: { name: 'USD Placeholder', symbol: 'USD', decimals: 18 },
    rpcUrls: {
        default: { http: ['https://go.getblock.io/<ACCESS-TOKEN>/'] }
    },
    blockExplorers: { default: { name: 'Tempo Explorer', url: 'https://explore.tempo.xyz' } }
});

const client = createPublicClient({
    chain: tempo,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({ method: 'eth_unsubscribe', params: ["0x9cef478923ff08bf67fde6c64013158d"] });
console.log(result);
```
{% endtab %}
{% endtabs %}
