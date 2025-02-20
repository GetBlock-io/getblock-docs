---
description: >-
  The voteUnsubscribe RPC Solana method allows clients to unsubscribe from vote
  notifications.
---

# voteUnsubscribe – Solana

{% hint style="success" %}
This method is useful for efficiently managing WebSocket subscriptions and preventing unnecessary updates from being received once they are no longer required.
{% endhint %}

#### Supported Networks

* **Mainnet**
* **Devnet**

#### Parameters

* **integer (required)**: The subscription ID to cancel. This ID is obtained from a previously established subscription and is necessary to identify which subscription should be removed.

#### Result

Returns a boolean value indicating whether the unsubscription was successful.

**Result Format**

* **bool**: `true` if successfully unsubscribed, `false` otherwise.

#### Request Example

**API Endpoints**

```json
wss://go.getblock.io/<ACCESS-TOKEN>/
```

**JSON-RPC Request**

{% tabs %}
{% tab title="wss" %}
```json
wscat -c "wss://go.getblock.io/<ACCESS-TOKEN>/" --exec '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "voteUnsubscribe",
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

Common **voteUnsubscribe** error scenarios:

* **Invalid subscription ID**: If the given subscription ID does not exist or is already unsubscribed.
* **Network issues**: Connectivity problems with the Solana JSON-RPC API.
* **Server overload**: If too many unsubscription requests are made within a short period, the server may limit API requests.

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

The **Solana voteUnsubscribe** method is useful for:

* Managing WebSocket connections efficiently by terminating unnecessary subscriptions.
* Reducing bandwidth consumption and improving application responsiveness.
* Optimizing blockchain tracking by focusing only on required updates.
* Ensuring that applications remain within API rate limits by properly managing subscriptions.

#### Code voteUnsubscribe Example – Web3 Integration



{% tabs %}
{% tab title="JavaScript" %}
```json
const WebSocket = require('ws');

const ws = new WebSocket("wss://go.getblock.io/<ACCESS-TOKEN>/");

ws.on('open', () => {
  const payload = {
    jsonrpc: "2.0",
    id: 1,
    method: "voteUnsubscribe",
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

By integrating Web3 **voteUnsubscribe** into Solana's Core API, developers can:

* Optimize WebSocket communication by reducing unnecessary updates.
* Improve efficiency by selectively managing vote subscriptions.
* Ensure blockchain applications only receive relevant real-time data.
* Avoid unnecessary API calls and reduce resource usage in Web3 applications.
* Facilitate better scalability for dApps requiring real-time blockchain synchronization.

This method is a key component of the Core API, allowing developers to manage vote subscriptions effectively in Solana-based applications. By properly handling WebSocket connections, developers can ensure smoother performance, enhanced reliability, and a more responsive user experience.
