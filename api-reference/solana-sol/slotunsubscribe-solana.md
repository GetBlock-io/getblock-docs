---
description: >-
  The slotUnsubscribe RPC Solana method allows clients to unsubscribe from
  receiving notifications about processed slots. This method is essential for
  managing Web3 slotUnsubscribe subscriptions.
---

# slotUnsubscribe – Solana

{% hint style="success" %}
The **slotUnsubscribe** method cancels an existing subscription to slot notifications previously created using **slotSubscribe RPC Solana**.
{% endhint %}

It is used to optimize resource consumption by **removing unnecessary data streams** from applications tracking slot changes.

### Supported Networks

* Mainnet

### Parameters

* **`integer`** (required): The **subscription ID** to cancel.

### Result

Returns a **`boolean` value** indicating whether the **unsubscribe request** was successful.

#### Result Format

* **`true`**: Successfully unsubscribed.
* **`false`**: Unsubscription failed.

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
  "method": "slotUnsubscribe",
  "params": [0]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful **request** returns a **confirmation message**.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": true,
  "id": 1
}
```

In this response:

* **`result`**: **`true`** confirms that the **subscription was successfully removed**.

### Error Handling

Common **slotUnsubscribe error** scenarios:

* **Invalid subscription ID**: The specified **subscription ID does not exist** or is already unsubscribed.
* **Network issues**: Problems with the **Solana JSON-RPC API endpoints**.

#### Example Error Response

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

### Use Cases

The **Solana slotUnsubscribe** method is essential for:

* **Managing real-time blockchain tracking**.
* **Optimizing API request handling**.
* **Efficiently controlling Web3 applications that monitor slot updates**.
* **Reducing unnecessary block or transaction tracking**.

### Code slotUnsubscribe Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket("wss://go.getblock.io/<ACCESS-TOKEN>/");

ws.on('open', () => {
  const payload = {
    jsonrpc: "2.0",
    id: 1,
    method: "slotUnsubscribe",
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

### Integration with Web3

By integrating **slotUnsubscribe** into **Solana's Core API**, developers can:

* **Optimize transaction tracking**.
* **Manage Web3 subscriptions efficiently**.
* **Ensure blockchain applications only receive relevant data**.
* **Improve overall application performance by limiting redundant requests**.

This method is a critical component of the **Core API**, allowing developers to handle **block updates, transaction tracking, and API requests** efficiently.
