---
description: >-
  Example code for the eth_subscribe json-rpc method. Сomplete guide on how to
  use eth_subscribe json-rpc in GetBlock.io Web3 documentation.
---

# eth\_subscribe - Optimism

This method creates a subscription that pushes data to the client via WebSocket when events occur. Subscription types include `'newHeads'` for new blocks, `'logs'` for log events, `'newPendingTransactions'` for pending transactions, and `'syncing'` for sync status changes. This provides more efficient real-time updates compared to polling.

{% hint style="warning" %}
The **eth\_subscribe** method creates a WebSocket subscription for real-time events on Optimism.
{% endhint %}

### Parameters

| Parameter        | Type   | Description                                                                               | Required |
| ---------------- | ------ | ----------------------------------------------------------------------------------------- | -------- |
| subscriptionType | String | Type of subscription: `'newHeads'`, `'logs'`, `'newPendingTransactions'`, or `'syncing'`. | Yes      |
| filterObject     | Object | (Optional, for `'logs'`) Filter object with address and topics.                           | No       |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL (wscat)" overflow="wrap" %}
```bash
# This method requires WebSocket connection
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

> {"jsonrpc":"2.0","method":"eth_subscribe","params":["newheads"],"id":"getblock.io"}
```
{% endcode %}
{% endtab %}

{% tab title="Javascript" %}
{% code title="JavaScript (ws)" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

ws.on('open', () => {
    ws.send(JSON.stringify({
        jsonrpc: '2.0',
        method: 'eth_subscribe',
        params: ['newHeads'],
        id: 'getblock.io'
    }));
});

ws.on('message', (data) => {
    console.log(JSON.parse(data));
});
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="Python (websockets)" %}
```python
import asyncio
import websockets
import json

async def unsubscribe():
    uri = "wss://go.getblock.io/<ACCESS-TOKEN>/"
    async with websockets.connect(uri) as ws:
        payload = {
            "jsonrpc": "2.0",
            "method": "eth_subscribe",
            "params": ["newHeads"],
            "id": "getblock.io"
        }
        await ws.send(json.dumps(payload))
        response = await ws.recv()
        print(json.loads(response))

asyncio.run(unsubscribe())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust (tokio-tungstenite)" %}
```rust
use tokio_tungstenite::connect_async;
use futures_util::{SinkExt, StreamExt};
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let url = "wss://go.getblock.io/<ACCESS-TOKEN>/";
    let (mut ws, _) = connect_async(url).await?;
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_subscribe",
        "params": ["newHeads"],
        "id": "getblock.io"
    });
    
    ws.send(payload.to_string().into()).await?;
    
    if let Some(msg) = ws.next().await {
        println!("{:?}", msg?);
    }
    
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x9ce59a13059e417087c02d3236a0b1cc"
}
```

### Response Parameters

| Field Name                  | Data Type      | Description                                                                                                                                                                                               |
| --------------------------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **subscription**            | string         | The subscription ID                                                                                                                                                                                       |
| **object**                  | object         | A block object containing the following fields:                                                                                                                                                           |
| **object.number**           | string or null | The block number of the requested block, encoded as hexadecimal. `null` if the block is pending                                                                                                           |
| **object.hash**             | string or null | The block hash of the requested block. `null` if the block is pending                                                                                                                                     |
| **object.parenthash**       | string         | Hash of the previous block used to generate the current block. Also known as the 'parent block'                                                                                                           |
| **object.nonce**            | string or null | The hash used to demonstrate proof-of-work. `null` if the block is pending. Returns `0x0000000000000000` when the consensus is proof-of-stake                                                             |
| **object.sha3uncles**       | string         | The hash of the list of uncles included in the block. Used to identify the block uniquely and to verify the integrity of the block's data                                                                 |
| **object.logsbloom**        | string or null | The bloom filter for the logs of the block, a data structure that allows for efficient membership testing of elements in a set (the logs included in the block). `null` if pending                        |
| **object.transactionsroot** | string         | The root of the transaction trie of the block. Allows Arbitrum nodes to verify the integrity of the transactions in a block                                                                               |
| **object.stateroot**        | string         | The root of the final state trie of the block. Included in the block header and used to verify the integrity of the state at the time the block was processed                                             |
| **object.receiptsroot**     | string         | The root of the receipts trie of the block. A 32-byte hash of the root node of the receipts trie of all transactions in the block. Used to verify the integrity of the receipts data for all transactions |
| **object.miner**            | string         | The address of the miner receiving the reward                                                                                                                                                             |
| **object.difficulty**       | string         | A measure of how hard it is to find a valid block for the Arbitrum blockchain. A number that increases as more miners join the network and more blocks are added to the chain, encoded as hexadecimal     |
| **object.totaldifficulty**  | string         | The cumulative sum of the difficulty of all blocks that have been mined in the Arbitrum network since the inception of the network. Measures the overall security and integrity of the Arbitrum network   |
| **object.extradata**        | string         | Extra data included in a block by the miner who mined it. Often includes messages or other information related to the block                                                                               |
| **object.size**             | string         | The size of this block in bytes as an integer value, encoded as hexadecimal                                                                                                                               |
| **object.gaslimit**         | string         | The maximum gas allowed in this block, encoded as hexadecimal                                                                                                                                             |
| **object.gasused**          | string         | The total used gas by all transactions in this block, encoded as hexadecimal                                                                                                                              |
| **object.timestamp**        | string         | The Unix timestamp for when the block was collated                                                                                                                                                        |
| **object.transactions**     | array          | An array of transaction objects. See eth\_getTransactionByHash for the exact shape                                                                                                                        |
| **object.uncles**           | array          | An array of uncle hashes                                                                                                                                                                                  |

### Use Case

The eth\_subscribe method is commonly used for:

* **Real-time block updates**
* **Live event streaming**
* **Efficient chain monitoring**
* **WebSocket applications**

### Error handling

| Status Code | Error Message              | Cause                             |
| ----------- | -------------------------- | --------------------------------- |
| 404         | Not found                  | Missing or invalid ACCESS\_TOKEN. |
| -32601      | notification not supported |                                   |

