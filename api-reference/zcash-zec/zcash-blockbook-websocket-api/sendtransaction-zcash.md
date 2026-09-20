---
description: >-
  Example code for the sendTransaction WebSocket method. Complete guide on how to use
  the sendTransaction WebSocket method in the GetBlock Web3 documentation.
---

# sendTransaction - Zcash

Broadcasts a signed, serialized transaction to the Zcash network and returns its transaction id on acceptance. This is the WebSocket form of the REST [api/v2/sendtx](../zcash-blockbook-rest-api/api-v2-sendtx-zcash.md) endpoint.

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
        "hex": "050000800a27a726..."
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": {
        "result": "076a0119b89f661efa34e815079726b36b4cfbd0375f66af72f505c88bafde37"
    }
}
```

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| result | string | Transaction id of the accepted transaction |

{% hint style="warning" %}
Acceptance means the node admitted the transaction to its mempool, not that it has been mined. A Zcash transaction also carries an `expiryheight`; once the chain passes it the transaction can never be mined and must be rebuilt rather than rebroadcast.

There is no fee estimator on Zcash. Size the fee from ZIP-317: 5,000 zatoshis per logical action, minimum two actions. The transaction must also be signed for the consensus branch ID that [getInfo](getinfo-zcash.md) reports.
{% endhint %}

## Use Cases

* **Wallet Sends**: Broadcast a transaction built and signed on the client
* **Payment Settlement**: Push a payout to the network
* **Shielding**: Broadcast a transaction moving transparent funds into a shielded pool

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| error | Transaction decode failed | The hex is malformed or not a complete signed transaction |
| error | Transaction rejected | The fee is below the ZIP-317 minimum, the expiry height has passed, or the branch ID is wrong |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
