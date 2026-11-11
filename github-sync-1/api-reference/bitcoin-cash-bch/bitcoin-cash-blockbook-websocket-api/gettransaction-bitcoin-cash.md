---
description: >-
  Example code for the getTransaction WebSocket method. Complete guide on how to use
  the getTransaction WebSocket method in the GetBlock Web3 documentation.
---

# getTransaction - Bitcoin Cash

Returns a normalized transaction by its id, with inputs, outputs, addresses, values, and confirmation data. This is the WebSocket form of the REST [api/v2/tx](../bitcoin-cash-blockbook-rest-api/api-v2-tx-bitcoin-cash.md) endpoint.

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| txid | string | Yes | Transaction id |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>

# then send:
{
    "id": "getblock.io",
    "method": "getTransaction",
    "params": {
        "txid": "94dec6ad13dd53825c75e280108669ff5b0658ebbd1acd6081e39e4bf620a235"
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": {
        "txid": "94dec6ad13dd53825c75e280108669ff5b0658ebbd1acd6081e39e4bf620a235",
        "version": 2,
        "vin": [
            {
                "txid": "4535b98aa154ccf1ffce7b094f4f2e375c3e9763ef927fc7b5933ffb954b2518",
                "vout": 1,
                "sequence": 4294967295,
                "n": 0,
                "addresses": ["bitcoincash:qzhfp027fncvmnguc5ue6h5rflmda5ptcu33duxphz"],
                "isAddress": true,
                "value": "20295459"
            }
        ],
        "vout": [
            {
                "value": "10699462",
                "n": 0,
                "hex": "76a91476a04053bda0a88bda5177b86a15c3b29f55987388ac",
                "addresses": ["bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a"],
                "isAddress": true
            }
        ],
        "blockHash": "0000000000000000022381149f45490d46624e874f0e078ac79478e69ae2f38d",
        "blockHeight": 961549,
        "confirmations": 7446,
        "blockTime": 1785230352,
        "value": "20294931",
        "valueIn": "20295459",
        "fees": "528"
    }
}
```

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| txid | string | The transaction id |
| vin | array | Inputs with addresses and values |
| vout | array | Outputs with values, scripts, and destination addresses |
| blockHeight | integer | Block height, or -1 while unconfirmed |
| confirmations | integer | Number of confirmations. 0 while unconfirmed |
| blockTime | integer | Block timestamp, or first-seen time for a mempool transaction |
| value | string | Total output value in satoshis |
| valueIn | string | Total input value in satoshis |
| fees | string | Fee paid in satoshis |

{% hint style="info" %}
Bitcoin Cash has no SegWit, so transactions carry no `vsize` or witness data and the response reports `size` only. Fee rates are therefore quoted per byte rather than per virtual byte.
{% endhint %}

## Use Cases

* **Payment Verification**: Confirm the amount and destination of a deposit
* **Confirmation Checks**: Read the current depth of a transaction
* **Fee Inspection**: Read the computed fee for a transaction
* **Input Tracing**: Follow the addresses a transaction spends

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| error | Not found | No transaction exists with the requested id |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
