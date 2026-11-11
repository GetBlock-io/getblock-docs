---
description: >-
  Example code for the getAccountUtxo WebSocket method. Complete guide on how to use
  the getAccountUtxo WebSocket method in the GetBlock Web3 documentation.
---

# getAccountUtxo - Bitcoin Cash

Returns the unspent transaction outputs for an address, extended public key, or descriptor over WebSocket. This is the WebSocket form of the REST [api/v2/utxo](../bitcoin-cash-blockbook-rest-api/api-v2-utxo-bitcoin-cash.md) endpoint.

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
        "descriptor": "bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a"
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
            "txid": "94dec6ad13dd53825c75e280108669ff5b0658ebbd1acd6081e39e4bf620a235",
            "vout": 0,
            "value": "10699462",
            "height": 961549,
            "confirmations": 7446
        },
        {
            "txid": "fde470024107b71206c7e658703d80b3c26c41e8c4782fd2b7d59c325e7723d4",
            "vout": 0,
            "value": "9000",
            "height": 961029,
            "confirmations": 7966
        }
    ]
}
```

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| txid | string | Transaction id of the output |
| vout | integer | Output index within the transaction |
| value | string | Output value in satoshis |
| height | integer | Block height at which the output was confirmed |
| confirmations | integer | Number of confirmations. 0 for unconfirmed outputs |
| address | string | Owning address, for xpub and descriptor queries only |
| path | string | Derivation path of the owning address, for xpub and descriptor queries only |

{% hint style="warning" %}
This method returns every unspent output in one message with no paging. A heavily used address can return thousands of entries: the account above holds over 11,000. Budget for the payload size, and prefer the REST endpoint with `confirmed=true` when only settled outputs are needed.
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
