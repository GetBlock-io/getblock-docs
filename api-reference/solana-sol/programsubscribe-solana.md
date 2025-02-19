---
description: >-
  The programSubscribe JSON-RPC method allows clients to subscribe to
  notifications about accounts owned by a specified program ID.
---

# programSubscribe – Solana

{% hint style="success" %}
The programSubscribe RPC Solana method listens for changes to lamports or data associated with program-owned accounts.&#x20;
{% endhint %}

Notifications are sent whenever account information is updated.

### Supported Networks

* Mainnet
* Devnet

### Parameters

#### Required Parameters

* string (required): Pubkey of the program ID (base-58 encoded).

#### Optional Parameters

* object (optional): Configuration object containing:
  * commitment (string): Commitment level.
    * Default: finalized
  * filters (array): An array of filter objects for account data.
    * dataSize: Filter accounts with data size in bytes.
    * memcmp: Filter accounts by matching data at a specific offset.
  * encoding (string): Account data encoding format.
    * Supported values: base58, base64, base64+zstd, jsonParsed.

### Result

The response returns a subscription ID.

#### Result Format

* integer: The subscription ID.

### Request Examples

#### API Endpoints

```json
wss://go.getblock.io/<ACCESS-TOKEN>/
```

#### JSON-RPC Request – Subscribe to Program with Base64 Encoding

{% tabs %}
{% tab title="wss" %}
```json
wscat -c "wss://go.getblock.io/<ACCESS-TOKEN>/" --exec '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "programSubscribe",
  "params": [
    "11111111111111111111111111111111",
    {
      "encoding": "base64",
      "commitment": "finalized"
    }
  ]
}'
```
{% endtab %}
{% endtabs %}

#### JSON-RPC Request – Subscribe to Program with JSON-Parsed Encoding

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "programSubscribe",
  "params": [
    "11111111111111111111111111111111",
    {
      "encoding": "jsonParsed"
    }
  ]
}
```

#### JSON-RPC Request – Subscribe to Program with Data Size Filter

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "programSubscribe",
  "params": [
    "11111111111111111111111111111111",
    {
      "encoding": "base64",
      "filters": [
        {
          "dataSize": 80
        }
      ]
    }
  ]
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

* result: The subscription ID.

### Notification Format

Notifications are sent as JSON-RPC responses containing account data.

#### Base58 Encoding Example

```json
{
  "jsonrpc": "2.0",
  "method": "programNotification",
  "params": {
    "result": {
      "context": {
        "slot": 5208469
      },
      "value": {
        "pubkey": "H4vnBqifaSACnKa7acsxstsY1iV1bvJNxsCY7enrd1hq",
        "account": {
          "data": [
            "11116bv5nS2h3y12kD1yUKeMZvGcKLSjQgX6BeV7u1FrjeJcKfsHPXHRDEHrBesJhZyqnnq9qJeUuF7WHxiuLuL5twc38w2TXNLxnDbjmuR",
            "base58"
          ],
          "executable": false,
          "lamports": 33594,
          "owner": "11111111111111111111111111111111",
          "rentEpoch": 636,
          "space": 80
        }
      }
    },
    "subscription": 24040
  }
}


```

#### Parsed-JSON Encoding Example

```json
{
  "jsonrpc": "2.0",
  "method": "programNotification",
  "params": {
    "result": {
      "context": {
        "slot": 5208469
      },
      "value": {
        "pubkey": "H4vnBqifaSACnKa7acsxstsY1iV1bvJNxsCY7enrd1hq",
        "account": {
          "data": {
            "program": "nonce",
            "parsed": {
              "type": "initialized",
              "info": {
                "authority": "Bbqg1M4YVVfbhEzwA9SpC9FhsaG83YMTYoR4a8oTDLX",
                "blockhash": "LUaQTmM7WbMRiATdMMHaRGakPtCkc2GHtH57STKXs6k",
                "feeCalculator": {
                  "lamportsPerSignature": 5000
                }
              }
            }
          },
          "executable": false,
          "lamports": 33594,
          "owner": "11111111111111111111111111111111",
          "rentEpoch": 636,
          "space": 80
        }
      }
    },
    "subscription": 24040
  }
}
```

### Error Handling

Common programSubscribe error scenarios:

* Invalid program ID: Incorrect Pubkey.
* Unsupported encoding: Invalid encoding type.
* Filter misconfiguration: Incorrect filter definitions.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid program ID"
  },
  "id": 1
}
```

### Use Cases

The Solana programSubscribe method is essential for:

* Monitoring accounts owned by programs.
* Tracking data changes for real-time analytics.
* Building dashboards for account state tracking.

### Code Example – Web3 programSubscribe Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const WebSocket = require('ws');

const url = "wss://go.getblock.io/<ACCESS-TOKEN>/";
const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "programSubscribe",
  params: [
    "11111111111111111111111111111111",
    {
      encoding: "base64",
      commitment: "finalized"
    }
  ]
};

const ws = new WebSocket(url);

ws.on('open', () => {
  ws.send(JSON.stringify(payload));
});

ws.on('message', (data) => {
  console.log("Program Subscription Update:", JSON.parse(data));
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

Integrating programSubscribe into Solana's Core API allows developers to:

* Receive real-time account updates.
* Filter notifications based on custom criteria.

Optimize dApp performance through selective data streams.
