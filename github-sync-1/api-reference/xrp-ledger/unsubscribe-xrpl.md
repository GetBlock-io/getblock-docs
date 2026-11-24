---
description: >-
  Example code for the unsubscribe JSON RPC method. Complete guide to using
  unsubscribe JSON-RPC in the GetBlock Web3 documentation.
---

# unsubscribe - XRPL

This method tells the server to stop sending messages for a particular subscription.

## Parameters

| Parameter          | Type  | Required | Description                  |
| ------------------ | ----- | -------- | ---------------------------- |
| streams            | array | No       | Streams to unsubscribe       |
| accounts           | array | No       | Accounts to stop watching    |
| accounts\_proposed | array | No       | Proposed tx accounts         |
| books              | array | No       | Order books to stop watching |

## Request Example

{% hint style="info" %}
`unsubscribe` works over WebSocket only, on the same connection that made the subscription. Connect to `wss://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>`.
{% endhint %}

{% tabs %}
{% tab title="wscat" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>

# After it connects, subscribe:
{"id": 1, "command": "subscribe", "streams": ["ledger"]}

# Then, on the same connection, unsubscribe:
{"id": 2, "command": "unsubscribe", "streams": ["ledger"]}
```
{% endtab %}

{% tab title="JavaScript (ws)" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket('wss://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>');

ws.on('open', () => {
    ws.send(JSON.stringify({ id: 1, command: 'subscribe', streams: ['ledger'] }));
});

ws.on('message', (data) => {
    const message = JSON.parse(data);
    console.log(message);

    // After the first ledger update, stop the subscription and close.
    if (message.type === 'ledgerClosed') {
        ws.send(JSON.stringify({ id: 2, command: 'unsubscribe', streams: ['ledger'] }));
    }
    if (message.id === 2) {
        ws.close();
    }
});
```
{% endtab %}
{% endtabs %}

## Response Example

```json
{
    "result": {
        "status": "success"
    }
}
```

## Response Parameters

| Parameter | Type   | Description        |
| --------- | ------ | ------------------ |
| status    | string | Unsubscribe status |

## Use Cases

* Stop receiving updates
* Clean up connections
* Resource management

## Error Handling

| Error Code    | Description        |
| ------------- | ------------------ |
| invalidParams | Invalid parameters |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
{% code title="xrpl.js (Node.js)" %}
```javascript
const xrpl = require('xrpl');

async function unsubscribe() {
    const client = new xrpl.Client('wss://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>');
    await client.connect();
    
    await client.request({
        command: 'unsubscribe',
        streams: ['ledger']
    });
    
    console.log('Unsubscribed');
    await client.disconnect();
}

unsubscribe();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl-py" %}
```python
from xrpl.clients import WebsocketClient
from xrpl.models.requests import Subscribe, Unsubscribe, StreamParameter

url = "wss://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>"

with WebsocketClient(url) as client:
    client.request(Subscribe(streams=[StreamParameter.LEDGER]))
    response = client.request(Unsubscribe(streams=[StreamParameter.LEDGER]))
    print(response.result)
```
{% endtab %}
{% endtabs %}
