---
description: >-
  The signatureSubscribe JSON-RPC method allows clients to subscribe to a single
  notification when a transaction with the provided signature reaches the
  specified commitment level.
---

# signatureSubscribe – Solana

{% hint style="info" %}
The signatureSubscribe RPC Solana method tracks the status of a transaction signature and notifies the client when it has been processed or received.&#x20;
{% endhint %}

This is crucial for dApps and Web3 applications that need real-time transaction status updates.

### Supported Networks

* Mainnet
* Devnet

### Parameters

#### Required Parameter

* string: Transaction signature (Base-58 encoded). This must be the first signature from the transaction.

#### Optional Parameter

* commitment (string): Defines the commitment level (finalized, confirmed, processed).
* enableReceivedNotification (bool): If true, notifications will be sent when signatures are received.

### Result

The response returns an integer value which serves as the subscription ID.

#### Result Format

* integer: The subscription ID required for unsubscribing.

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
  "method": "signatureSubscribe",
  "params": [
    "2EBVM6cB8vAAD93Ktr6Vd8p67XPbQzCJX47MpReuiCXJAtcjaxpvWpcg9Ege1Nr5Tk3a2GFrByT7WPBjdsTycY9b",
    {
      "commitment": "finalized",
      "enableReceivedNotification": false
    }
  ]
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

The notification contains the transaction status or signature reception confirmation.

#### Example Notification (Processed Transaction)

```json
{
  "jsonrpc": "2.0",
  "method": "signatureNotification",
  "params": {
    "result": {
      "context": {
        "slot": 5207624
      },
      "value": {
        "err": null
      }
    },
    "subscription": 24006
  }
}
```

#### Example Notification (Received Signature)

```json
{
  "jsonrpc": "2.0",
  "method": "signatureNotification",
  "params": {
    "result": {
      "context": {
        "slot": 5207624
      },
      "value": "receivedSignature"
    },
    "subscription": 24006
  }
}
```

### Error Handling

Common signatureSubscribe error scenarios:

* Invalid transaction signature.
* Network issues.
* Unsupported configuration parameters.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "Invalid signature"
  },
  "id": 1
}
```

### Use Cases

The Solana signatureSubscribe method is essential for:

* Monitoring transaction status.
* Tracking real-time application interactions.
* Enhancing user experience in Web3 applications.

### Code signatureSubscribe Example – Web3 Integration

{% tabs %}
{% tab title="wss" %}
```json
const WebSocket = require('ws');

const url = "wss://go.getblock.io/<ACCESS-TOKEN>/";
const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "signatureSubscribe",
  params: [
    "2EBVM6cB8vAAD93Ktr6Vd8p67XPbQzCJX47MpReuiCXJAtcjaxpvWpcg9Ege1Nr5Tk3a2GFrByT7WPBjdsTycY9b",
    {
      commitment: "finalized",
      enableReceivedNotification: true
    }
  ]
};

const ws = new WebSocket(url);

ws.on('open', () => {
  ws.send(JSON.stringify(payload));
});

ws.on('message', (data) => {
  console.log("Signature Subscription Update:", JSON.parse(data));
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

By integrating Web3 signatureSubscribe into Solana's Core API, developers can:

* Track transaction status in real-time.
* Respond to user actions more effectively.

Optimize application reliability.
