---
description: >-
  Example code for the sendTransaction WebSocket method. Complete guide on how to use
  the sendTransaction WebSocket method in the GetBlock Web3 documentation.
---

# sendTransaction - Bitcoin Cash

Broadcasts a signed, serialized transaction to the Bitcoin Cash network and returns its transaction id on acceptance. This is the WebSocket form of the REST [api/v2/sendtx](../bitcoin-cash-blockbook-rest-api/api-v2-sendtx-bitcoin-cash.md) endpoint.

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
        "result": "94dec6ad13dd53825c75e280108669ff5b0658ebbd1acd6081e39e4bf620a235"
    }
}
```

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| result | string | Transaction id of the accepted transaction |

{% hint style="warning" %}
Acceptance means the node admitted the transaction to its mempool, not that it has been mined. Track it to the required depth with [subscribeNewBlock](subscribenewblock-bitcoin-cash.md) before treating a payment as settled.

Bitcoin Cash transactions must be signed with the BCH sighash algorithm (`SIGHASH_FORKID`). A transaction signed for Bitcoin will be rejected.
{% endhint %}

## Use Cases

* **Wallet Sends**: Broadcast a transaction built and signed on the client
* **Payment Settlement**: Push a payout to the network
* **Retry Flows**: Rebroadcast a transaction that has not been mined

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| error | Transaction decode failed | The hex is malformed or not a complete signed transaction |
| error | min relay fee not met | The fee is below the node's relay threshold |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
