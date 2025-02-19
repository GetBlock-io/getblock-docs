---
description: >-
  This method is essential for real-time monitoring of slot changes and
  blockchain synchronization in Web3 applications.
---

# slotsUpdatesSubscribe – Solana

{% hint style="success" %}
The **slotsUpdatesSubscribe RPC Solana** method allows clients to **subscribe** to receive notifications about various **slot updates** from the validator.
{% endhint %}

The **slotsUpdatesSubscribe** method provides a WebSocket-based **subscription** to track slot updates, including events like slot creation, finalization, or optimistic confirmations. Developers use this to monitor **block, transaction**, and **slot-related events** efficiently.

### Supported Networks

* **Mainnet**
* **Devnet**

### Parameters

{% hint style="info" %}
**None**
{% endhint %}

### Result

Returns a **subscription ID**, which is required for **unsubscribing** from notifications.

#### Result Format

* **integer**: The **subscription ID** needed to **unsubscribe**.

### Request Example

#### API Endpoints

```
wss://go.getblock.io/<ACCESS-TOKEN>/
```

#### JSON-RPC Request

{% tabs %}
{% tab title="wss" %}
```json
wscat -c "wss://go.getblock.io/<ACCESS-TOKEN>/" --exec '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "slotsUpdatesSubscribe"
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
  "method": "slotsUpdatesNotification",
  "params": {
    "result": {
      "parent": 75,
      "slot": 76,
      "timestamp": 1625081266243,
      "type": "optimisticConfirmation"
    },
    "subscription": 0
  }
}
```

#### Notification Fields

* **parent** _(optional)_: The **parent slot** (present for "createdBank" events).
* **slot**: The **updated slot**.
* **timestamp**: Unix timestamp (milliseconds) of the update.
* **type**: Update type, which can be one of:
  * `firstShredReceived`
  * `completed`
  * `createdBank`
  * `frozen`
  * `dead`
  * `optimisticConfirmation`
  * `root`
* **stats** _(optional)_: Present for "frozen" updates, containing transaction statistics:
  * `maxTransactionsPerEntry`
  * `numFailedTransactions`
  * `numSuccessfulTransactions`
  * `numTransactionEntries`
* **err** _(optional)_: Error message, only for "dead" updates.

### Error Handling

Common **slotsUpdatesSubscribe error** scenarios:

* **Network issues**: Problems with the **Solana JSON-RPC API endpoints**.
* **Invalid WebSocket connection**: Issues with **maintaining a stable connection**.
* **Unsupported subscription**: Some **nodes** may not support this method due to its unstable nature.

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

The **Solana slotsUpdatesSubscribe** method is essential for:

* **Live blockchain monitoring**.
* **Tracking slot status in real-time**.
* **Keeping applications in sync** with the latest slot updates.
* **Improving dApp responsiveness by reacting to slot changes.**

### Code Example – Web3 slotsUpdatesSubscribe Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket("wss://go.getblock.io/<ACCESS-TOKEN>/");

ws.on('open', () => {
  const payload = {
    jsonrpc: "2.0",
    id: 1,
    method: "slotsUpdatesSubscribe"
  };
  ws.send(JSON.stringify(payload));
});

ws.on('message', (data) => {
  console.log("Slots Updates Notification:", JSON.parse(data));
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

By integrating **slotsUpdatesSubscribe** into **Solana's Core API**, developers can:

* **Monitor slot progression in real-time**.
* **Enhance dApps with live updates**.
* **Improve blockchain data tracking systems**.
* **Track important block and transaction updates.**

This method is a critical component of the **Core API**, allowing developers to handle **block updates, transaction tracking, and API requests** efficiently.
