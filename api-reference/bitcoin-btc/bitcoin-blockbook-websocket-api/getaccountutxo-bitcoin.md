---
description: >-
  Example code for the getAccountUtxo WebSocket method. Complete guide on how to
  use the getAccountUtxo WebSocket method in the GetBlock Web3 documentation.
---

# getAccountUtxo - Bitcoin

Returns the unspent transaction outputs for an address, extended public key, or descriptor over WebSocket. These outputs are the inputs available when constructing a spending transaction. This is the WebSocket form of the REST [api/v2/utxo](../bitcoin-blockbook-rest-api/api-v2-utxo-bitcoin.md) endpoint.

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
            "txid": "1d51c696eab35d7dad7631516cc0845aaab317fe67cb182782752547541d30ea",
            "vout": 0,
            "value": "32323",
            "height": 967397,
            "confirmations": 91
        },
        {
            "txid": "a6494142e2e565b5e672d41a37a3eafec2fe5594f22efbb07f421a6cedf473c5",
            "vout": 1,
            "value": "100000",
            "height": 966724,
            "confirmations": 764
        }
    ]
}
```

As with the REST endpoint, an address query returns no `address` or `path`; querying an xpub or descriptor adds both, identifying which derived address owns each output.

## Response Fields

| Field         | Type    | Description                                                              |
| ------------- | ------- | ------------------------------------------------------------------------ |
| txid          | string  | Transaction id of the output                                             |
| vout          | integer | Output index within the transaction                                      |
| value         | string  | Output value in satoshis                                                 |
| height        | integer | Block height at which the output was confirmed. Omitted when unconfirmed |
| confirmations | integer | Number of confirmations. 0 for unconfirmed outputs                       |
| address       | string  | Address that owns the output, for xpub and descriptor queries            |
| path          | string  | Derivation path of the owning address, for xpub and descriptor queries   |

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
