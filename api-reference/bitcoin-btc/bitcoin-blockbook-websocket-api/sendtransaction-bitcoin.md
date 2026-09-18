---
description: >-
  Example code for the sendTransaction WebSocket method. Complete guide on how
  to use the sendTransaction WebSocket method in the GetBlock Web3
  documentation.
---

# sendTransaction - Bitcoin

Broadcasts a signed, serialized transaction to the Bitcoin network through the backend node and returns its transaction id on acceptance. This is the WebSocket form of the REST [api/v2/sendtx](../bitcoin-blockbook-rest-api/api-v2-sendtx-bitcoin.md) endpoint.

## Parameters

| Parameter | Type   | Required | Description                               |
| --------- | ------ | -------- | ----------------------------------------- |
| hex       | string | Yes      | Hex-encoded signed raw transaction        |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/websocket

# then send:
{
    "id": "getblock.io",
    "method": "sendTransaction",
    "params": {
        "hex": "02000000000101662a2fc609ddb4eb..."
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": {
        "result": "7c3be24063f268aaa1ed81b64776798f56088757641a34fb156c4f51ed2e9d25"
    }
}
```

## Response Fields

| Field  | Type   | Description                                       |
| ------ | ------ | ------------------------------------------------- |
| result | string | Transaction id of the accepted transaction        |

## Use Cases

* **Wallet Sends**: Broadcast a transaction built and signed on the client
* **Payment Settlement**: Push a payout to the network
* **Consolidation**: Broadcast a transaction combining several small outputs
* **Retry Flows**: Rebroadcast a transaction that has not been mined

{% hint style="warning" %}
Acceptance means the backend node admitted the transaction to its mempool, not that it has been mined. Track it to the required depth with [subscribeAddresses](subscribeaddresses-bitcoin.md) or [subscribeNewBlock](subscribenewblock-bitcoin.md) before treating a payment as settled.
{% endhint %}

## Error Handling

| Error                     | Message                     | Description                                                       |
| ------------------------- | --------------------------- | ------------------------------------------------------------------- |
| error                     | Transaction decode failed   | The hex is malformed or not a complete signed transaction         |
| error                     | Transaction already in block chain | The transaction has already been mined                     |
| error                     | min relay fee not met       | The fee is below the backend node's relay threshold               |
| 403 / RBAC: access denied | Access denied               | The GetBlock access token is missing or incorrect                 |
