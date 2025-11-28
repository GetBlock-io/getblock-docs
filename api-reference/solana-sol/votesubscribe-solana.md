---
description: >-
  The voteSubscribe RPC Solana method allows clients to subscribe to
  notifications whenever a new vote is observed in gossip.
---

# voteSubscribe – Solana

{% hint style="success" %}
These votes are **pre-consensus**, meaning there is no guarantee they will enter the ledger. This method enables real-time monitoring of voting activity on the Solana network, making it useful for tracking validator behavior and network governance.
{% endhint %}

#### Unstable Method

This subscription is **unstable** and only available if the validator was started with the `--rpc-pubsub-enable-vote-subscription` flag. The format of this subscription may change in future Solana updates.

#### Supported Networks

* Mainnet

#### Parameters

{% hint style="info" %}
None: This method does not require any parameters.
{% endhint %}

#### Result

Returns an integer representing the subscription ID, which is required for unsubscribing.

**Result Format**

* **`integer`**: The subscription ID.

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
  "method": "voteSubscribe"
}'
```
{% endtab %}
{% endtabs %}

#### Response

A successful request returns a subscription ID.

**Example Response**

```json
{
  "jsonrpc": "2.0",
  "result": 0,
  "id": 1
}
```

In this response:

* **result**: `0` represents the assigned subscription ID.

#### Notification Format

Upon vote updates, clients receive notifications containing vote details.

**Example Notification**

```json
{
  "jsonrpc": "2.0",
  "method": "voteNotification",
  "params": {
    "result": {
      "hash": "8Rshv2oMkPu5E4opXTRyuyBeZBqQ4S477VG26wUTFxUM",
      "slots": [1, 2],
      "timestamp": null
    },
    "subscription": 0
  }
}
```

#### Notification Fields

* **`hash`** (_string_): The vote hash.
* **`slots`** (_array_): The slots covered by the vote, represented as an array of `u64` integers.
* **`timestamp`** (_i64|null_): The timestamp of the vote. It may be `null` if unavailable.
* **`signature`** (_string_): The signature of the transaction that contained this vote.
* **`votePubkey`** (_string_): The public key of the vote account, encoded in base-58.

#### Error Handling

Common **voteSubscribe** error scenarios:

* **Network issues**: Problems with the Solana JSON-RPC API endpoints.
* **Invalid WebSocket connection**: Issues maintaining a stable connection.
* **Subscription failure**: If the validator was not started with the required flag.

**Example Error Response**

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

#### Use Cases

The **Solana voteSubscribe** method is useful for:

* Tracking real-time validator votes in the Solana network.
* Monitoring voting behavior for governance and security purposes.
* Analyzing vote distribution across slots.
* Building dApps that require real-time voting updates.
* Ensuring applications stay in sync with validator activities.

#### Code voteSubscribe Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket("wss://go.getblock.io/<ACCESS-TOKEN>/");

ws.on('open', () => {
  const payload = {
    jsonrpc: "2.0",
    id: 1,
    method: "voteSubscribe"
  };
  ws.send(JSON.stringify(payload));
});

ws.on('message', (data) => {
  console.log("Vote Notification:", JSON.parse(data));
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

By integrating Web3 **voteSubscribe** into Solana's Core API, developers can:

* Monitor validator votes in real-time.
* Improve blockchain tracking systems.
* Enhance dApps with live vote updates.
* Ensure applications remain synchronized with validator activities.
* Optimize governance tools by tracking consensus formation.

This method is a critical component of the Core API that enables efficient vote tracking, request handling, and transaction validation in Solana-based applications.
