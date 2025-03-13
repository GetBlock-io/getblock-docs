---
description: >-
  The signatureUnsubscribe JSON-RPC method allows clients to unsubscribe from
  signature confirmation notifications.
---

# signatureUnsubscribe – Solana

{% hint style="success" %}
The **signatureUnsubscribe** RPC Solana method is used to cancel an active signature subscription.&#x20;
{% endhint %}

When a subscription ID is provided, the server will stop sending notifications for the corresponding transaction signature.

### Supported Networks

* Mainnet
* Devnet

### Parameters

#### Required Parameter

* **`number`**: The subscription ID to cancel.

### Result

The response returns a boolean value indicating the status of the unsubscribe operation.

#### Result Format

* `bool`: `true` if the unsubscribe was successful; otherwise, `false`.

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
  "method": "signatureUnsubscribe",
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

* `result`: `true` indicates the unsubscribe operation was successful.

### Error Handling

Common signatureUnsubscribe error scenarios:

* **Invalid subscription ID**: The provided ID does not match an active subscription.
* **Network issues**: Problems with the Solana JSON-RPC API endpoints.
* **signatureunsubscribe error:** invalid subscription id. This error occurs if an invalid ID is passed to the RPC call.

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

The Solana signatureUnsubscribe method is essential for:

* Managing network resources by terminating unnecessary subscriptions.
* Optimizing dApp performance.
* Reducing network bandwidth consumption.

### Code signatureUnsubscribe Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const WebSocket = require('ws');

const url = "wss://go.getblock.io/<ACCESS-TOKEN>/";
const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "signatureUnsubscribe",
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

By integrating Web3 signatureUnsubscribe into Solana's Core API, developers can:

* Manage subscriptions more effectively.
* Reduce network congestion.

Optimize application performance.
