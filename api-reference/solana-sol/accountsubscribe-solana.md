---
description: >-
  The accountSubscribe JSON-RPC method enables clients to subscribe to updates
  about a specific Solana account.
---

# accountSubscribe – Solana

{% hint style="success" %}
The accountSubscribe RPC Solana method is often used in dApps to track account balance changes and state modifications.&#x20;
{% endhint %}

It provides a subscription ID that can be used to unsubscribe later. The notification format is similar to the getAccountInfo method's response.

### Supported Networks

* Mainnet
* Devnet

### Parameters

#### Required Parameters

* string (required): The account Pubkey as a base-58 encoded string.

#### Optional Parameters

* object (optional): A configuration object containing:
  * commitment (string): Commitment level to observe.
  * encoding (string): The encoding format for the account data.
    * Supported: base58, base64, base64+zstd, jsonParsed.

### Result

The response returns a subscription ID required to unsubscribe from the account notifications.

#### Result Format

* number: The subscription ID.

### Request Example

#### API Endpoints

```json
wss://go.getblock.io/<ACCESS-TOKEN>/
```

#### JSON-RPC Request

{% tabs %}
{% tab title="curl" %}
```json
wscat -c "wss://go.getblock.io/<ACCESS-TOKEN>/" -x '{
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

### Response

A successful request returns a subscription ID.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": 23784,
  "id": 1
}
```

In this response:

* result: The subscription ID assigned to this account.

### Notification Formats

#### Base58 Encoding

```json
{
  "jsonrpc": "2.0",
  "method": "accountNotification",
  "params": {
    "result": {
      "context": {
        "slot": 5199307
      },
      "value": {
        "data": [
          "11116bv5nS2h3y12kD1yUKeMZvGcKLSjQgX6BeV7u1FrjeJcKfsHPXHRDEHrBesJhZyqnnq9qJeUuF7WHxiuLuL5twc38w2TXNLxnDbjmuR",
          "base58"
        ],
        "executable": false,
        "lamports": 33594,
        "owner": "11111111111111111111111111111111",
        "rentEpoch": 635,
        "space": 80
      }
    },
    "subscription": 23784
  }
}
```

#### Parsed JSON Encoding

```json
{
  "jsonrpc": "2.0",
  "method": "accountNotification",
  "params": {
    "result": {
      "context": {
        "slot": 5199307
      },
      "value": {
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
        "rentEpoch": 635,
        "space": 80
      }
    }
  }
}
```

### Error Handling

Common accountSubscribe error scenarios:

* Invalid Pubkey: Incorrect account Pubkey.
* Unsupported encoding: Invalid encoding format.
* Network issues: Connectivity problems with Solana JSON-RPC API endpoints.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid Pubkey"
  },
  "id": 1
}
```

### Use Cases

The Solana accountSubscribe method is essential for:

* Real-time account balance monitoring.
* Tracking smart contract state changes.
* Enabling event-driven dApp functionality.

### Code accountSubscribe Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket("wss://go.getblock.io/<ACCESS-TOKEN>/");

ws.on('open', () => {
  const payload = {
    jsonrpc: "2.0",
    id: 1,
    method: "accountSubscribe",
    params: [
      "CM78CPUeXjn8o3yroDHxUtKsZZgoy4GPkPPXfouKNH12",
      {
        encoding: "jsonParsed",
        commitment: "finalized"
      }
    ]
  };
  ws.send(JSON.stringify(payload));
});

ws.on('message', (data) => {
  console.log("Subscription Update:", JSON.parse(data));
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

By integrating Web3 accountSubscribe into Solana's Core API, developers can monitor account changes, optimize dApp performance, and enable responsive features. This method is vital for Web3 applications that depend on real-time data updates.
