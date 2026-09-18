---
description: >-
  Example code for the getAccountUtxo WebSocket method. Complete guide on how to
  use the getAccountUtxo WebSocket method in the GetBlock Web3 documentation.
---

# getAccountUtxo - Bitcoin

Returns the unspent transaction outputs for an address, extended public key, or descriptor over WebSocket. These outputs are the inputs available when constructing a spending transaction. This is the WebSocket form of the REST [api/v2/utxo](../bitcoin-blockbook-rest-api/api-v2-utxo-bitcoin.md) endpoint.

## Parameters

| Parameter  | Type   | Required | Description                                                         |
| ---------- | ------ | -------- | ------------------------------------------------------------------- |
| descriptor | string | Yes      | Address, extended public key, or descriptor to read outputs for     |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/websocket

# then send:
{
    "id": "getblock.io",
    "method": "getAccountUtxo",
    "params": {
        "descriptor": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq"
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
            "txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
            "vout": 0,
            "value": "50000000",
            "height": 830000,
            "confirmations": 152,
            "address": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq",
            "path": "m/84'/0'/0'/0/0"
        }
    ]
}
```

## Response Fields

| Field         | Type    | Description                                                            |
| ------------- | ------- | ----------------------------------------------------------------------- |
| txid          | string  | Transaction id of the output                                            |
| vout          | integer | Output index within the transaction                                     |
| value         | string  | Output value in satoshis                                                |
| height        | integer | Block height at which the output was confirmed. Omitted when unconfirmed |
| confirmations | integer | Number of confirmations. 0 for unconfirmed outputs                      |
| address       | string  | Address that owns the output, for xpub and descriptor queries           |
| path          | string  | Derivation path of the owning address, for xpub and descriptor queries  |

## Use Cases

* **Coin Selection**: Read spendable outputs when building a transaction
* **Balance Construction**: Sum output values to compute a spendable balance
* **Wallet Funding**: Find the UTXOs behind a wallet before spending
* **Consolidation**: Identify small outputs worth combining

{% hint style="info" %}
Unlike the REST endpoint, this method has no `confirmed` parameter: both confirmed and unconfirmed outputs are returned. Filter on `confirmations` in the client, or use [api/v2/utxo](../bitcoin-blockbook-rest-api/api-v2-utxo-bitcoin.md) with `confirmed=true` when only settled outputs are wanted.
{% endhint %}

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| error                     | Invalid address | The descriptor is malformed                       |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
