---
description: >-
  Example code for the sendTransaction WebSocket method. Complete guide on how to use
  the sendTransaction WebSocket method in the GetBlock Web3 documentation.
---

# sendTransaction - Litecoin

Broadcasts a signed, serialized transaction to the Litecoin network and returns its transaction id on acceptance. This is the WebSocket form of the REST [api/v2/sendtx](../litecoin-blockbook-rest-api/api-v2-sendtx-litecoin.md) endpoint.

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| hex | string | Yes | Hex-encoded signed raw transaction |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/websocket

# then send:
{
    "id": "getblock.io",
    "method": "sendTransaction",
    "params": {
        "hex": "0200000001..."
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": {
        "result": "51a31c4236a99bfd17cda606f079c9c4814851119e7593ab7c739bd5972b1969"
    }
}
```

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| result | string | Transaction id of the accepted transaction |

{% hint style="warning" %}
Acceptance means the node admitted the transaction to its mempool, not that it has been mined. Track it to the required depth with [subscribeNewBlock](subscribenewblock-litecoin.md) before treating a payment as settled.
{% endhint %}

## Use Cases

* **Wallet Sends**: Broadcast a transaction built and signed on the client
* **Payment Settlement**: Push a payout to the network
* **Consolidation**: Broadcast a transaction combining several small outputs
* **Retry Flows**: Rebroadcast a transaction that has not been mined

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| error | Transaction decode failed | The hex is malformed or not a complete signed transaction |
| error | min relay fee not met | The fee is below the node's relay threshold |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
