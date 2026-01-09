---
description: >-
  Example code for the suix_subscribeEvent JSON-RPC method. Complete guide on
  how to use suix_subscribeEvent JSON-RPC in GetBlock Web3 documentation.
---

# suix\_subscribeEvent - Sui

This method subscribes to a stream of SUI events matching specified filter criteria via WebSocket. This enables real-time event monitoring for applications that need to react immediately to on-chain activities like DeFi trades, NFT mints, or smart contract state changes.

{% hint style="info" %}
This method requires a WebSocket connection, not HTTP.
{% endhint %}

### Parameters

| Parameter | Type        | Required | Description           |
| --------- | ----------- | -------- | --------------------- |
| filter    | EventFilter | Yes      | Event filter criteria |

#### EventFilter Options

* `All` - Match all events
* `Transaction` - Match by transaction digest
* `MoveModule` - Match by package and module
* `MoveEventType` - Match by event type
* `MoveEventModule` - Match by event module
* `MoveEventField` - Match by field value
* `Sender` - Match by sender address
* `TimeRange` - Match by timestamp range

### Request Example

{% tabs %}
{% tab title="JavaScript (ws)" %}
{% code title="subscribe.js" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

ws.on('open', () => {
  const subscribeRequest = {
    jsonrpc: '2.0',
    id: 1,
    method: 'suix_subscribeEvent',
    params: [
      {
        MoveModule: {
          package: '0x2',
          module: 'coin'
        }
      }
    ]
  };
  ws.send(JSON.stringify(subscribeRequest));
});

ws.on('message', (data) => {
  const response = JSON.parse(data);
  console.log('Received:', response);
});
```
{% endcode %}
{% endtab %}

{% tab title="Python (websockets)" %}
{% code title="subscribe.py" %}
```python
import asyncio
import websockets
import json

async def subscribe_events():
    uri = "wss://go.getblock.io/<ACCESS-TOKEN>/"
    async with websockets.connect(uri) as websocket:
        request = {
            "jsonrpc": "2.0",
            "id": 1,
            "method": "suix_subscribeEvent",
            "params": [{
                "MoveModule": {
                    "package": "0x2",
                    "module": "coin"
                }
            }]
        }
        await websocket.send(json.dumps(request))
        
        while True:
            response = await websocket.recv()
            print(json.loads(response))

asyncio.run(subscribe_events())
```
{% endcode %}
{% endtab %}

{% tab title="Rust (tokio-tungstenite)" %}
{% code title="main.rs" %}
```rust
use tokio_tungstenite::connect_async;
use futures_util::{StreamExt, SinkExt};
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let (mut ws_stream, _) = connect_async("wss://go.getblock.io/<ACCESS-TOKEN>/").await?;
    
    let request = json!({
        "jsonrpc": "2.0",
        "id": 1,
        "method": "suix_subscribeEvent",
        "params": [{"MoveModule": {"package": "0x2", "module": "coin"}}]
    });
    
    ws_stream.send(request.to_string().into()).await?;
    
    while let Some(msg) = ws_stream.next().await {
        println!("{:?}", msg?);
    }
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response Example

{% tabs %}
{% tab title="Subscription Confirmation" %}
```json
{
  "jsonrpc": "2.0",
  "result": 3087420193638836,
  "id": 1
}
```
{% endtab %}

{% tab title="Event Notification" %}
```json
{
  "jsonrpc": "2.0",
  "method": "suix_subscribeEvent",
  "params": {
    "subscription": 3087420193638836,
    "result": {
      "id": {
        "txDigest": "FFwCMgC7FHBLEwfL9JeSeR2EhMAZMykUPVW1kE3HgTMe",
        "eventSeq": "1"
      },
      "packageId": "0x2",
      "transactionModule": "coin",
      "sender": "0xcceee09f44d558691334ec0aff47af033f57162a2f33056e2585e2c46863ac02",
      "type": "0x2::coin::CoinCreated",
      "parsedJson": {}
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Response Parameters

| Parameter    | Type   | Description                   |
| ------------ | ------ | ----------------------------- |
| subscription | number | Subscription ID               |
| result       | object | Event data when events arrive |

### Use Cases

* Real-time DeFi price feeds
* Live NFT activity monitoring
* Transaction notifications
* Smart contract event tracking
* Building live dashboards

### Error Handling

| Error Code        | Description            |
| ----------------- | ---------------------- |
| -32602            | Invalid filter format  |
| -32603            | Internal error         |
| Connection closed | WebSocket disconnected |

### SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="typescript-example.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });

const unsubscribe = await client.subscribeEvent({
  filter: { MoveModule: { package: '0x2', module: 'coin' } },
  onMessage: (event) => {
    console.log('Event received:', event);
  }
});

// Later: unsubscribe();
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="rust-example.rs" %}
```rust
use sui_sdk::SuiClientBuilder;
use sui_sdk::rpc_types::EventFilter;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default()
        .ws_url("wss://go.getblock.io/<ACCESS-TOKEN>/")
        .build("https://go.getblock.io/<ACCESS-TOKEN>/")
        .await?;
    
    let mut subscribe = sui.event_api().subscribe_event(
        EventFilter::MoveModule { package: "0x2".parse()?, module: "coin".into() }
    ).await?;
    
    while let Some(event) = subscribe.next().await {
        println!("{:?}", event?);
    }
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
