---
description: >-
  Example code for the suix_subscribeTransaction JSON-RPC method. Complete guide
  on how to use suix_subscribeTransaction JSON-RPC in GetBlock Web3
  documentation.
---

# suix\_subscribeTransaction - Sui

This method subscribes to a stream of transaction effects matching specified filter criteria via WebSocket. This enables real-time transaction monitoring for applications tracking specific addresses, objects, or transaction types.

{% hint style="info" %}
This method requires a WebSocket connection, not HTTP.
{% endhint %}

## Parameters

| Parameter | Type              | Required | Description                 |
| --------- | ----------------- | -------- | --------------------------- |
| filter    | TransactionFilter | Yes      | Transaction filter criteria |

### TransactionFilter Options

* `Checkpoint` - Match by checkpoint
* `MoveFunction` - Match by Move function call
* `InputObject` - Match by input object
* `ChangedObject` - Match by modified object
* `FromAddress` - Match by sender
* `ToAddress` - Match by recipient
* `FromAndToAddress` - Match by both sender and recipient
* `TransactionKind` - Match by transaction type

## Request Example

{% tabs %}
{% tab title="JavaScript (WebSocket)" %}
{% code title="subscribe.js" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

ws.on('open', () => {
  const subscribeRequest = {
    jsonrpc: '2.0',
    id: 1,
    method: 'suix_subscribeTransaction',
    params: [
      {
        FromAddress: '0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961'
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

async def subscribe_transactions():
    uri = "wss://go.getblock.io/<ACCESS-TOKEN>/"
    async with websockets.connect(uri) as websocket:
        request = {
            "jsonrpc": "2.0",
            "id": 1,
            "method": "suix_subscribeTransaction",
            "params": [{
                "FromAddress": "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961"
            }]
        }
        await websocket.send(json.dumps(request))
        
        while True:
            response = await websocket.recv()
            print(json.loads(response))

asyncio.run(subscribe_transactions())
```
{% endcode %}
{% endtab %}

{% tab title="Rust (tokio-tungstenite)" %}
{% code title="subscribe.rs" %}
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
        "method": "suix_subscribeTransaction",
        "params": [{"FromAddress": "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961"}]
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

## Response Example

{% tabs %}
{% tab title="Transaction Notification" %}
```json
{
  "jsonrpc": "2.0",
  "result": 3087420193638837,
  "id": 1
}
```
{% endtab %}

{% tab title="Subscription Confirmation" %}
```json
{
  "jsonrpc": "2.0",
  "method": "suix_subscribeTransaction",
  "params": {
    "subscription": 3087420193638837,
    "result": {
      "effects": {
        "messageVersion": "v1",
        "status": { "status": "success" },
        "transactionDigest": "7dp5WtTmtGp83EXYYFMzjBJRFeSgR67AzqMETLrfgeFx"
      },
      "timestamp": "1676911928000"
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Response Parameters

| Parameter    | Type   | Description                                 |
| ------------ | ------ | ------------------------------------------- |
| subscription | number | Subscription ID                             |
| result       | object | Transaction effects when transactions match |

## Use Cases

* Monitor wallet activity in real-time
* Track object modifications
* Build payment notifications
* Watch smart contract interactions
* Create transaction alerts

## Error Handling

| Error Code        | Description            |
| ----------------- | ---------------------- |
| -32602            | Invalid filter format  |
| -32603            | Internal error         |
| Connection closed | WebSocket disconnected |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="sdk-typescript.ts" overflow="wrap" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });

const unsubscribe = await client.subscribeTransaction({
  filter: { FromAddress: '0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961' },
  onMessage: (tx) => {
    console.log('Transaction received:', tx);
  }
});

// Later: unsubscribe();
```
{% endcode %}
{% endtab %}

{% tab title="SUI Rust SDK" %}
{% code title="sdk-rust.rs" overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;
use sui_sdk::rpc_types::TransactionFilter;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default()
        .ws_url("wss://go.getblock.io/<ACCESS-TOKEN>/")
        .build("https://go.getblock.io/<ACCESS-TOKEN>/")
        .await?;
    
    let mut subscribe = sui.read_api().subscribe_transaction(
        TransactionFilter::FromAddress("0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961".parse()?)
    ).await?;
    
    while let Some(tx) = subscribe.next().await {
        println!("{:?}", tx?);
    }
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
