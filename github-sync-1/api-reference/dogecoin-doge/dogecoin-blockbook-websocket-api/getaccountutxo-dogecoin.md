---
description: >-
  Example code for the getAccountUtxo WebSocket method. Complete guide on how to use
  the getAccountUtxo WebSocket method in the GetBlock Web3 documentation.
---

# getAccountUtxo - Dogecoin

Returns the unspent transaction outputs for an address, extended public key, or descriptor over WebSocket. This is the WebSocket form of the REST [api/v2/utxo](../dogecoin-blockbook-rest-api/api-v2-utxo-dogecoin.md) endpoint.

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| descriptor | string | Yes | Address, extended public key, or descriptor to read outputs for |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>

# then send:
{
    "id": "getblock.io",
    "method": "getAccountUtxo",
    "params": {
        "descriptor": "DRv9o4XUK1DhNiuKPadPwkQNkPgtcniBFF"
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
            "txid": "4c5519699240ed1f23e1ec46ea4bc85f019982f6ef48afdb7698054230a4b76e",
            "vout": 0,
            "value": "1000000",
            "height": 6378960,
            "confirmations": 5
        },
        {
            "txid": "8d99a37f27d702b35bcc74dfea0d1a006a67c20571e1de660a30defc16316c8f",
            "vout": 1,
            "value": "248941280401251",
            "height": 6378956,
            "confirmations": 9
        }
    ]
}
```

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| txid | string | Transaction id of the output |
| vout | integer | Output index within the transaction |
| value | string | Output value in koinu |
| height | integer | Block height at which the output was confirmed |
| confirmations | integer | Number of confirmations. 0 for unconfirmed outputs |
| address | string | Owning address, for xpub and descriptor queries only |
| path | string | Derivation path of the owning address, for xpub and descriptor queries only |

{% hint style="info" %}
Unlike the REST endpoint, this method has no `confirmed` parameter: both confirmed and unconfirmed outputs are returned, and there is no paging. The account above holds 585 outputs in a single message. Filter on `confirmations` in the client, or use REST with `confirmed=true` when only settled outputs are wanted.
{% endhint %}

## Use Cases

* **Coin Selection**: Read spendable outputs when building a transaction
* **Balance Construction**: Sum output values to compute a spendable balance
* **Consolidation**: Identify small outputs worth combining

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| error | Invalid address | The descriptor is malformed |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
