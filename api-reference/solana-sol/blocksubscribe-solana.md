---
description: >-
  The blockSubscribe JSON-RPC method allows clients to subscribe to block
  notifications. This method triggers a notification whenever a new block is
  confirmed or finalized.
---

# blockSubscribe – Solana

{% hint style="danger" %}
#### Unstable Method

This method is considered unstable and requires the validator to be started with the --rpc-pubsub-enable-block-subscription flag. Future changes to this method's format are possible.
{% endhint %}

The blockSubscribe RPC Solana method provides real-time block notifications based on specified filters. Developers can track all transactions or only those related to a specific account or program.

### Supported Networks

* Mainnet
* Devnet

### Parameters

#### Required Parameters

* filter (string | object): Defines filter criteria for the notifications:
  * string: all – Receive notifications for all blocks.
  * object: A JSON object containing:
    * mentionsAccountOrProgram: A base-58 encoded string of an account or program.

#### Optional Parameters

* object (optional): Configuration object containing:
  * commitment (string): Commitment level for block finality.
    * Default: finalized
  * encoding (string): Transaction encoding format.
    * Default: json
    * Supported: json, jsonParsed, base58, base64
  * transactionDetails (string): Level of transaction detail.
    * Default: full
    * Supported: full, accounts, signatures, none
  * maxSupportedTransactionVersion (number): The maximum transaction version to return.
  * showRewards (bool): Include block rewards.
    * Default: true

### Result

The response returns a subscription ID.

#### Result Format

* integer: The subscription ID.

### Request Examples

#### API Endpoints

```json
wss://go.getblock.io/<ACCESS-TOKEN>/
```

#### JSON-RPC Request – Subscribe to All Blocks

{% tabs %}
{% tab title="wss" %}
```json
wscat -c "wss://go.getblock.io/<ACCESS-TOKEN>/" -x '{
  "jsonrpc": "2.0",
  "id": "1",
  "method": "blockSubscribe",
  "params": ["all"]
}'
```
{% endtab %}
{% endtabs %}

#### JSON-RPC Request – Subscribe to Specific Account/Program

```json
{
  "jsonrpc": "2.0",
  "id": "1",
  "method": "blockSubscribe",
  "params": [
    {
      "mentionsAccountOrProgram": "LieKvPRE8XeX3Y2xVNHjKlpAScD12lYySBVQ4HqoJ5op"
    },
    {
      "commitment": "confirmed",
      "encoding": "base64",
      "showRewards": true,
      "transactionDetails": "full"
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
  "result": 0,
  "id": 1
}
```

In this response:

* result: The subscription ID.

### Error Handling

Common blockSubscribe error scenarios:

* Invalid filter: Incorrect filter string or JSON object.
* Unsupported encoding: Invalid encoding type.
* Network issues: Problems with the Solana JSON-RPC API endpoints.

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

The Solana blockSubscribe method is essential for:

* Real-time block monitoring.
* dApps requiring live transaction data.
* Blockchain explorers tracking new blocks.
* Performance analytics.

### Code blockSubscribe Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```json
const WebSocket = require('ws');

const url = "Endpoint";
const payload = {
  jsonrpc: "2.0",
  id: "1",
  method: "blockSubscribe",
  params: [
    {
      mentionsAccountOrProgram: "LieKvPRE8XeX3Y2xVNHjKlpAScD12lYySBVQ4HqoJ5op"
    },
    {
      commitment: "confirmed",
      encoding: "base64",
      showRewards: true,
      transactionDetails: "full"
    }
  ]
};

const ws = new WebSocket(url);

ws.on('open', () => {
  ws.send(JSON.stringify(payload));
});

ws.on('message', (data) => {
  console.log("Block Subscription Update:", JSON.parse(data));
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

Integrating Web3 blockSubscribe into Solana's Core API allows developers to:

* Receive block notifications for all blocks or specific accounts/programs.
* Enhance real-time monitoring in blockchain applications.

Utilize transaction details for data analytics and performance insights.
