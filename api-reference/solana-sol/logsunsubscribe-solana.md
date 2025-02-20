---
description: >-
  The logsUnsubscribe JSON-RPC method allows clients to unsubscribe from
  receiving transaction log notifications.
---

# logsUnsubscribe – Solana

{% hint style="success" %}
The logsUnsubscribe RPC Solana method cancels an active logs subscription using its subscription ID.
{% endhint %}

Once unsubscribed, the client will no longer receive notifications about transaction logs.

### Supported Networks

* Mainnet
* Devnet

### Parameters

#### Required Parameter

* integer (required): The subscription ID for the logs subscription to cancel.

### Result

The response returns a boolean value indicating whether the unsubscribe operation was successful.

#### Result Format

* bool: true if the unsubscribe was successful; otherwise, false.

### Request Example

#### API Endpoints

```json
wss://go.getblock.io/<ACCESS-TOKEN>/
```

#### JSON-RPC Request

{% tabs %}
{% tab title="wss" %}
```json
wscat -c "wss://go.getblock.io/<ACCESS-TOKEN>/" --exec '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "logsUnsubscribe",
  "params": [0]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns a boolean value.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": true,
  "id": 1
}
```

In this response:

* result: true indicates the unsubscribe operation was successful.

### Error Handling

Common logsUnsubscribe error scenarios:

* Invalid subscription ID: The provided ID does not correspond to an active subscription.
* Network issues: Problems with the Solana JSON-RPC API endpoints.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid subscription ID"
  },
  "id": 1
}
```

### Use Cases

The Solana logsUnsubscribe method is essential for:

* Managing network resources by terminating unused subscriptions.
* Optimizing dApp performance.
* Reducing network bandwidth consumption.

### Code logsUnsubscribe Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const WebSocket = require('ws');

const url = "wss://go.getblock.io/<ACCESS-TOKEN>/";
const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "logsUnsubscribe",
  params: [0]
};

const ws = new WebSocket(url);

ws.on('open', () => {
  ws.send(JSON.stringify(payload));
});

ws.on('message', (data) => {
  console.log("Unsubscribe Response:", JSON.parse(data));
});

ws.on('error', (error) => {
  console.error("WebSocket error:", error.message);
});

ws.on('close', () => {
  console.log("WebSocket connection closed");
});
```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 logsUnsubscribe into Solana's Core API, developers can efficiently manage subscription lifecycles, improve dApp performance, and optimize network resource utilization.
