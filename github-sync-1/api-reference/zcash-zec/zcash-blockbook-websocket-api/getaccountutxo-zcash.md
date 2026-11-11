---
description: >-
  Example code for the getAccountUtxo WebSocket method. Complete guide on how to use
  the getAccountUtxo WebSocket method in the GetBlock Web3 documentation.
---

# getAccountUtxo - Zcash

Returns the unspent transparent outputs for an address, extended public key, or descriptor over WebSocket. This is the WebSocket form of the REST [api/v2/utxo](../zcash-blockbook-rest-api/api-v2-utxo-zcash.md) endpoint.

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| descriptor | string | Yes | Transparent address, extended public key, or descriptor |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>

# then send:
{
    "id": "getblock.io",
    "method": "getAccountUtxo",
    "params": {
        "descriptor": "t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua"
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": [
        {
            "txid": "873df9973be48d17d305ec078fdea2c248c9a0e5b592c67b5909fd0a9280cc9f",
            "vout": 0,
            "value": "116166174",
            "height": 3489837,
            "confirmations": 1
        }
    ]
}
```

The array is truncated to one entry; the account above holds 179 outputs.

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| txid | string | Transaction id of the output |
| vout | integer | Output index within the transaction |
| value | string | Output value in zatoshis |
| height | integer | Block height at which the output was confirmed |
| confirmations | integer | Number of confirmations. 0 for unconfirmed outputs |
| address | string | Owning address, for xpub and descriptor queries only |
| path | string | Derivation path of the owning address, for xpub and descriptor queries only |

{% hint style="info" %}
This method has no `confirmed` parameter and no paging: every unspent transparent output is returned in one message. Filter on `confirmations` in the client, or use REST with `confirmed=true` when only settled outputs are wanted.
{% endhint %}

## Use Cases

* **Coin Selection**: Read spendable transparent outputs when building a transaction
* **Balance Construction**: Sum output values to compute a spendable transparent balance

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| error | Invalid address | The descriptor is malformed |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
