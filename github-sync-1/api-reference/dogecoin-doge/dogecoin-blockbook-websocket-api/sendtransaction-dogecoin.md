---
description: >-
  Example code for the sendTransaction WebSocket method. Complete guide on how to use
  the sendTransaction WebSocket method in the GetBlock Web3 documentation.
---

# sendTransaction - Dogecoin

Broadcasts a signed, serialized transaction to the Dogecoin network and returns its transaction id on acceptance. This is the WebSocket form of the REST [api/v2/sendtx](../dogecoin-blockbook-rest-api/api-v2-sendtx-dogecoin.md) endpoint.

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| hex | string | Yes | Hex-encoded signed raw transaction |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>

# then send:
{
    "id": "getblock.io",
    "method": "sendTransaction",
    "params": {
        "hex": "0100000001..."
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": {
        "result": "d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed"
    }
}
```

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| result | string | Transaction id of the accepted transaction |

{% hint style="warning" %}
Acceptance means the node admitted the transaction to its mempool, not that it has been mined. Track it to the required depth with [subscribeNewBlock](subscribenewblock-dogecoin.md) before treating a payment as settled.

Dogecoin's relay minimum is high relative to other UTXO chains. Size the fee from [estimateFee](estimatefee-dogecoin.md), and check the estimate is positive first: a one-block target returns a negative rate.
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
