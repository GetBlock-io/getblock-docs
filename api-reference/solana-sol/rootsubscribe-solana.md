---
description: >-
  The rootSubscribe JSON-RPC method enables clients to subscribe to
  notifications when a new root slot is set by the Solana validator.
---

# rootSubscribe – Solana

{% hint style="success" %}
The rootSubscribe RPC Solana method provides real-time notifications whenever the root slot changes.
{% endhint %}

In Solana's blockchain architecture, the root slot represents the most recent finalized block. This information helps applications maintain up-to-date state without polling the network.

### Supported Networks

* Mainnet
* Devnet

### Parameters

{% hint style="info" %}
None: This method does not require any parameters.
{% endhint %}

### Result

The response returns an integer value that serves as the subscription ID.

#### Result Format

* integer: The subscription ID to be used for unsubscribing.

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
  "method": "rootSubscribe"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns the subscription ID.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": 0,
  "id": 1
}
```

In this response:

* result: The subscription ID.

### Notification Format

Notifications are sent as JSON-RPC responses containing the latest root slot.

#### Example Notification

```json
{
  "jsonrpc": "2.0",
  "method": "rootNotification",
  "params": {
    "result": 42,
    "subscription": 0
  }
}
```

### Error Handling

Common rootSubscribe error scenarios:

* Network connectivity issues.
* Server-side configuration errors.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "Internal server error"
  },
  "id": 1
}
```

### Use Cases

The Solana rootSubscribe method is essential for:

* Monitoring network finality.
* Ensuring application state consistency.
* Developing real-time analytics tools.

### Code rootSubscribe Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const WebSocket = require('ws');

const url = "wss://go.getblock.io/<ACCESS-TOKEN>/";
const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "rootSubscribe"
};

const ws = new WebSocket(url);

ws.on('open', () => {
  ws.send(JSON.stringify(payload));
});

ws.on('message', (data) => {
  console.log("Root Subscription Update:", JSON.parse(data));
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

By integrating Web3 rootSubscribe into Solana's Core API, developers can:

* Monitor root slot changes in real-time.
* Optimize resource usage.

Build responsive and efficient dApps.
