---
description: >-
  Example code for the subscribe JSON RPC method. Complete guide to using
  subscribe JSON-RPC in the GetBlock Web3 documentation.
---

# subscribe - XPRL

This method requests periodic notifications from the server when certain events happen.

## Parameters

| Parameter          | Type  | Required | Description               |
| ------------------ | ----- | -------- | ------------------------- |
| streams            | array | No       | Streams to subscribe      |
| accounts           | array | No       | Accounts to watch         |
| accounts\_proposed | array | No       | Accounts for proposed txs |
| books              | array | No       | Order books to watch      |

## Request Example

{% hint style="info" %}
`subscribe` works over WebSocket only. Connect to `wss://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>` and keep the connection open to receive updates.
{% endhint %}

{% tabs %}
{% tab title="wscat" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>

# After it connects, send:
{"id": 1, "command": "subscribe", "streams": ["ledger"]}
```
{% endtab %}

{% tab title="JavaScript (ws)" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket('wss://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>');

ws.on('open', () => {
    ws.send(JSON.stringify({
        id: 1,
        command: 'subscribe',
        streams: ['ledger']
    }));
});

ws.on('message', (data) => {
    console.log(JSON.parse(data));
});
```
{% endtab %}
{% endtabs %}

## Response Example

```json
{
    "result": {
        "fee_base": 10,
        "fee_ref": 10,
        "ledger_hash": "3A0B41C31B13436610C5961E2C914529CAD3CCC4A7859ED6AABFCD0700847BC1",
        "ledger_index": 63632030,
        "ledger_time": 674581032,
        "reserve_base": 10000000,
        "reserve_inc": 2000000,
        "status": "success"
    }
}
```

## Response Parameters

| Parameter     | Type   | Description         |
| ------------- | ------ | ------------------- |
| ledger\_index | number | Current ledger      |
| status        | string | Subscription status |

## Use Cases

* Real-time ledger updates
* Monitor account activity
* Track order book changes

## Error Handling

| Error Code    | Description        |
| ------------- | ------------------ |
| invalidParams | Invalid parameters |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
```javascript
const xrpl = require('xrpl');

async function subscribe() {
    const client = new xrpl.Client('wss://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>');
    await client.connect();
    
    client.on('ledgerClosed', ledger => {
        console.log('New ledger:', ledger.ledger_index);
    });
    
    await client.request({
        command: 'subscribe',
        streams: ['ledger']
    });
}

subscribe();
```
{% endtab %}

{% tab title="xrpl-py" %}
```python
from xrpl.clients import WebsocketClient
from xrpl.models.requests import Subscribe, StreamParameter

url = "wss://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>"

with WebsocketClient(url) as client:
    client.send(Subscribe(streams=[StreamParameter.LEDGER]))
    for message in client:
        print(message)
```
{% endtab %}
{% endtabs %}
