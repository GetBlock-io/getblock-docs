---
description: >-
  Example code for the getAccountUtxo WebSocket method. Complete guide on how to
  use the getAccountUtxo WebSocket method in the GetBlock Web3 documentation.
---

# getAccountUtxo - Litecoin

Returns the unspent transaction outputs for an address, extended public key, or descriptor over WebSocket. This is the WebSocket form of the REST [api/v2/utxo](../litecoin-blockbook-rest-api/api-v2-utxo-litecoin.md) endpoint.

## Parameters

| Parameter  | Type   | Required | Description                                                     |
| ---------- | ------ | -------- | --------------------------------------------------------------- |
| descriptor | string | Yes      | Address, extended public key, or descriptor to read outputs for |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>

# then send:
{
    "id": "getblock.io",
    "method": "getAccountUtxo",
    "params": {
        "descriptor": "LMRmNEdhi5F43vtW5qPdv1q9s7xsMp4XK5"
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
            "txid": "51a31c4236a99bfd17cda606f079c9c4814851119e7593ab7c739bd5972b1969",
            "vout": 0,
            "value": "8324080358",
            "height": 3179800,
            "confirmations": 17
        },
        {
            "txid": "4c3d9548bb03a42551c10f2472e99cd86776ed00fd3bfb530c4466b93e7ddac7",
            "vout": 0,
            "value": "8080253532",
            "height": 3179209,
            "confirmations": 608
        }
    ]
}
```

## Response Fields

| Field         | Type    | Description                                                                 |
| ------------- | ------- | --------------------------------------------------------------------------- |
| txid          | string  | Transaction id of the output                                                |
| vout          | integer | Output index within the transaction                                         |
| value         | string  | Output value in litoshis                                                    |
| height        | integer | Block height at which the output was confirmed                              |
| confirmations | integer | Number of confirmations. 0 for unconfirmed outputs                          |
| address       | string  | Owning address, for xpub and descriptor queries only                        |
| path          | string  | Derivation path of the owning address, for xpub and descriptor queries only |

{% hint style="info" %}
Unlike the REST endpoint, this method has no `confirmed` parameter: both confirmed and unconfirmed outputs are returned. Filter on `confirmations` in the client, or use REST with `confirmed=true` when only settled outputs are wanted.
{% endhint %}

## Use Cases

* **Coin Selection**: Read spendable outputs when building a transaction
* **Balance Construction**: Sum output values to compute a spendable balance
* **Wallet Funding**: Find the UTXOs behind a wallet before spending
* **Consolidation**: Identify small outputs worth combining

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| error                     | Invalid address | The descriptor is malformed                       |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
