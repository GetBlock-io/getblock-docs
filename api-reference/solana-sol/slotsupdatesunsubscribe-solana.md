---
description: >-
  The slotsUpdatesUnsubscribe RPC Solana method allows clients to unsubscribe
  from slot-update notifications.
---

# slotsUpdatesUnsubscribe – Solana

{% hint style="info" %}
&#x20;This method is essential for managing WebSocket subscriptions efficiently, ensuring that applications do not receive unnecessary updates once they are no longer needed.
{% endhint %}

By using **slotsUpdatesUnsubscribe**, applications can prevent redundant data from being processed, reducing network traffic and improving performance. This is particularly useful in scenarios where real-time monitoring of blockchain slots is needed for analytics, transaction tracking, or validator operations.

#### Supported Networks

* **Mainnet**
* **Devnet**

#### Parameters

* **number (required)**: The subscription ID to cancel.

#### Result

Returns a boolean value indicating whether the unsubscription was successful.

**Result Format**

* **bool**: `true` if successfully unsubscribed, `false` otherwise.

#### Request Example

**API Endpoints**

```
wss://go.getblock.io/<ACCESS-TOKEN>/
```

**JSON-RPC Request**

{% tabs %}
{% tab title="wss" %}
```json
wscat -c "wss://go.getblock.io/<ACCESS-TOKEN>/" --exec '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "slotsUpdatesUnsubscribe",
  "params": [0]
}'
```
{% endtab %}
{% endtabs %}

#### Response

A successful request returns a confirmation of the unsubscription.

**Example Response**

```json
{
  "jsonrpc": "2.0",
  "result": true,
  "id": 1
}
```

In this response:

* **result**: `true` confirms that the subscription has been successfully removed.

#### Error Handling

Common **slotsUpdatesUnsubscribe** error scenarios:

* **Invalid subscription ID**: If the given subscription ID does not exist or is already unsubscribed.
* **Network issues**: Connectivity problems with the Solana JSON-RPC API.

**Example Error Response**

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "Invalid subscription ID"
  },
  "id": 1
}
```

#### Use Cases

The **Solana slotsUpdatesUnsubscribe** method is useful for:

* Managing WebSocket connections efficiently.
* Reducing unnecessary WebSocket traffic.
* Optimizing application performance by removing redundant slot update notifications.

#### Code Example – Web3 slotsUpdatesUnsubscribe Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket("wss://go.getblock.io/<ACCESS-TOKEN>/");

ws.on('open', () => {
  const payload = {
    jsonrpc: "2.0",
    id: 1,
    method: "slotsUpdatesUnsubscribe",
    params: [0]
  };
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

#### Integration with Web3

By integrating **slotsUpdatesUnsubscribe** into Solana's Core API, developers can:

* Optimize WebSocket communication.
* Free up resources by unsubscribing from unnecessary updates.
* Improve blockchain tracking efficiency.
* Ensure only relevant slot updates are received, enhancing application responsiveness.

This method is a key component of the Core API, allowing developers to manage block and transaction update subscriptions effectively in Solana-based applications.
