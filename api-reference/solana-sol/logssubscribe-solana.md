---
description: >-
  The logsSubscribe JSON-RPC method allows clients to subscribe to transaction
  logs on the Solana blockchain.
---

# logsSubscribe – Solana

{% hint style="success" %}
The logsSubscribe RPC Solana method provides notifications for transaction logs, filtered by account type.
{% endhint %}

Developers can track all transactions, vote transactions, or transactions mentioning specific accounts.

### Supported Networks

* Mainnet

### Parameters

#### Required Parameters

* **`filter`** (`string` | `object`): Defines filter criteria for transaction logs.
  * `string`: One of the following:
    * `all`: All transactions excluding vote transactions.
    * `allWithVotes`: All transactions including vote transactions.
  * `object`: A JSON object with:
    * `mentions`: An array containing a single Pubkey (as a base-58 encoded string).
      * Note: Only one Pubkey is supported; multiple Pubkeys will cause an error.

#### Optional Parameters

* **`object`** (optional): Configuration object containing:
  * `commitment` (`string`): Commitment level for transaction log subscription.

### Result

The response returns a subscription ID.

#### Result Format

* `integer`: The subscription ID.

### Request Examples

#### API Endpoints

```json
wss://go.getblock.io/<ACCESS-TOKEN>/
```

#### JSON-RPC Request – Subscribe to Logs by Mentioned Account

{% tabs %}
{% tab title="wss" %}
```json
wscat -c "wss://go.getblock.io/<ACCESS-TOKEN>/" --exec '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "accountSubscribe",
    "params": [
      "CM78CPUeXjn8o3yroDHxUtKsZZgoy4GPkPPXfouKNH12",
      {
        "encoding": "jsonParsed",
        "commitment": "finalized"
      }
    ]
}'
```
{% endtab %}
{% endtabs %}

#### JSON-RPC Request – Subscribe to All Logs

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "logsSubscribe",
  "params": ["all"]
}
```

### Response

A successful request returns the subscription ID.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": 24040,
  "id": 1
}
```

In this response:

* `result`: The subscription ID.

### Notification Format

Notifications are sent as JSON-RPC responses containing transaction log details.

#### Example Notification

```json
{
  "jsonrpc": "2.0",
  "method": "logsNotification",
  "params": {
    "result": {
      "context": {
        "slot": 5208469
      },
      "value": {
        "signature": "5h6xBEauJ3PK6SWCZ1PGjBvj8vDdWG3KpwATGy1ARAXFSDwt8GFXM7W5Ncn16wmqokgpiKRLuS83KUxyZyv2sUYv",
        "err": null,
        "logs": [
          "SBF program 83astBRguLMdt2h5U1Tpdq5tjFoJ6noeGwaY3mDLVcri success"
        ]
      }
    },
    "subscription": 24040
  }
}
```

### Error Handling

Common **logsSubscribe error** scenarios:

* **Invalid filter**: Incorrect filter string or JSON object.
* **Unsupported Pubkey array**: More than one Pubkey specified.
* **Network issues**: Problems with the Solana JSON-RPC API endpoints.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid filter criteria"
  },
  "id": 1
}
```

### Use Cases

The Solana **logsSubscribe** method is essential for:

* Monitoring smart contract activity.
* Tracking transactions mentioning specific accounts.
* Detecting errors during transaction execution.
* Real-time event streaming.

### Code logsSubscribe Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const WebSocket = require('ws');

const url = "wss://go.getblock.io/<ACCESS-TOKEN>/";
const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "logsSubscribe",
  params: [
    {
      mentions: ["11111111111111111111111111111111"]
    },
    {
      commitment: "finalized"
    }
  ]
};

const ws = new WebSocket(url);

ws.on('open', () => {
  ws.send(JSON.stringify(payload));
});

ws.on('message', (data) => {
  console.log("Logs Subscription Update:", JSON.parse(data));
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

Integrating Web3 logsSubscribe into Solana's Core API allows developers to:

* Track real-time logs for transactions.
* Monitor specific account activity.
* Optimize dApp performance by analyzing logs.

Detect errors early during transaction execution.
