---
description: >-
  The slotSubscribe RPC Solana method allows clients to subscribe to receive
  notifications whenever a slot is processed by the validator.
---

# slotSubscribe – Solana

{% hint style="info" %}
The **slotSubscribe RPC Solana** method establishes a **WebSocket connection** that notifies clients when a new **slot** is set.&#x20;
{% endhint %}

This is particularly useful for applications that require **live updates** on the Solana blockchain. Developers can use this method to **track transaction processing, blockchain synchronization, and block validation.**

### Supported Networks

* **Mainnet**
* **Devnet**

### Parameters

{% hint style="info" %}
None: This method does not require any parameters.
{% endhint %}

### Result

Returns a **subscription ID**, which is required for **unsubscribing** from notifications.

#### Result Format

* **integer**: The **subscription ID** needed to **unsubscribe**.

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
  "method": "slotSubscribe"
}'

```
{% endtab %}
{% endtabs %}

### Response

A successful **request** returns a **subscription ID**.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": 0,
  "id": 1
}
```

In this response:

* **result**: **0** represents the assigned **subscription ID**.

### Notification Format

Upon slot updates, clients receive notifications containing slot details.

#### Example Notification

```json
{
  "jsonrpc": "2.0",
  "method": "slotNotification",
  "params": {
    "result": {
      "parent": 75,
      "root": 44,
      "slot": 76
    },
    "subscription": 0
  }
}
```

#### Notification Fields

* **parent**: The **parent slot**.
* **root**: The **current root slot**.
* **slot**: The **newly set slot value**.

### Error Handling

Common **slotSubscribe error** scenarios:

* **Network issues**: Problems with the **Solana JSON-RPC API endpoints**.
* **Invalid WebSocket connection**: Issues with **maintaining a stable connection**.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "WebSocket connection error"
  },
  "id": 1
}
```

### Use Cases

The **Solana slotSubscribe** method is essential for:

* **Live blockchain monitoring**.
* **Tracking real-time slot updates** for validators and explorers.
* **Keeping applications in sync** with the Solana blockchain.
* **Enhancing dApps** by ensuring **transaction validation** and **block consistency**.

### Code slotSubscribe Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket("wss://go.getblock.io/<ACCESS-TOKEN>/");

ws.on('open', () => {
  const payload = {
    jsonrpc: "2.0",
    id: 1,
    method: "slotSubscribe"
  };
  ws.send(JSON.stringify(payload));
});

ws.on('message', (data) => {
  console.log("Slot Notification:", JSON.parse(data));
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

By integrating Web3 **slotSubscribe** into **Solana's Core API**, developers can:

* **Monitor slot progression in real-time**.
* **Enhance dApps with live updates**.
* **Improve blockchain data tracking systems**.
* **Ensure API requests** are processed efficiently.
* **Track block synchronization** to optimize transactions and **block validation**.

This method is a critical component of the **Core API** that enables efficient **block tracking, request handling, and transaction updates** in Solana-based applications.
