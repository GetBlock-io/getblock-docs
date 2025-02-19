---
description: >-
  The accountUnsubscribe JSON-RPC method allows clients to unsubscribe from
  receiving account change notifications.
---

# accountUnsubscribe – Solana

{% hint style="info" %}
The accountUnsubscribe RPC Solana method is part of the Solana JSON-RPC API.&#x20;
{% endhint %}

It cancels an active account subscription using its subscription ID. Once unsubscribed, the client will no longer receive notifications related to that account.

### Supported Networks

This method is accessible via the following API endpoints:

* Mainnet
* Devnet

### Parameters

#### Required Parameter

* number (required): The subscription ID of the account to unsubscribe.

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
wscat -c "wss://go.getblock.io/api-key" -x '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "accountUnsubscribe",
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

Common accountUnsubscribe error scenarios:

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

The Solana accountUnsubscribe method is essential for:

* Managing network resources by terminating unused subscriptions.
* Optimizing dApp performance.
* Reducing network bandwidth consumption.

### Code Example – Web3 accountUnsubscribe Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const WebSocket = require('ws');

const url = "wss://go.getblock.io/api-key";
const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "accountUnsubscribe",
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

By integrating accountUnsubscribe into Solana's Core API, developers can efficiently manage subscription lifecycles, improve dApp performance, and optimize network resource utilization.

\
