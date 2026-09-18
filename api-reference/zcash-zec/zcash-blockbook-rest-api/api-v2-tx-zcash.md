---
description: >-
  Example code for the api/v2/tx REST method. Complete guide on how to use the
  api/v2/tx REST method in the GetBlock Web3 documentation.
---

# api/v2/tx - Zcash

This endpoint returns a normalized transaction by its id, with transparent inputs, transparent outputs, addresses, values, and confirmation data in the indexer's unified schema. Shielded components are not represented; use [api/v2/tx-specific](api-v2-tx-specific-zcash.md) for those.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| txid | string | path | Yes | The transaction id |
| spending | boolean | query | No | Include spending transaction data for outputs when available |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx/80d21f43485fb5aed2bcb011a00fb70776b2f6b4ba05722dbf2dce6f53fc410f'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx/80d21f43485fb5aed2bcb011a00fb70776b2f6b4ba05722dbf2dce6f53fc410f'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx/80d21f43485fb5aed2bcb011a00fb70776b2f6b4ba05722dbf2dce6f53fc410f')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "txid": "80d21f43485fb5aed2bcb011a00fb70776b2f6b4ba05722dbf2dce6f53fc410f",
    "version": 4,
    "vin": [
        {
            "txid": "66ae9ab575911041815fc6afab7b74defb841f6cd04242ec9c993c7ad2652f36",
            "sequence": 4294967295,
            "n": 0,
            "addresses": [
                "t1Ne88F8ouCV92brDXNBB47a85brvnEHE8g"
            ],
            "isAddress": true,
            "value": "136544616339"
        }
    ],
    "vout": [
        {
            "value": "10000000000",
            "n": 0,
            "addresses": [
                "t1ZZEGyxzT34CKfAjZhMkA9fW5Fa6E1MfAx"
            ],
            "isAddress": true
        }
    ],
    "blockHash": "00000000000f3c0bdf81aba0bf081c5c4882975cbc0a8ce35e70ddb5f265f1b1",
    "blockHeight": 3487790,
    "confirmations": 46,
    "blockTime": 1789741929,
    "size": 893,
    "value": "546177605356",
    "valueIn": "546177745356",
    "fees": "140000"
}
```

The `vin` and `vout` arrays are truncated to one entry each above.

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| txid | string | The transaction id |
| version | integer | Transaction version: 4 for Sapling-era, 5 for NU5 and later |
| vin | array | Transparent inputs with addresses and values. Empty when all inputs are shielded |
| vout | array | Transparent outputs with values and destination addresses |
| blockHeight | integer | Block height, or -1 while unconfirmed |
| confirmations | integer | Number of confirmations. 0 while unconfirmed |
| size | integer | Serialized size in bytes |
| value | string | Total transparent output value in zatoshis |
| valueIn | string | Total transparent input value in zatoshis |
| fees | string | `valueIn` minus `value`. Correct only when the transaction has no shielded component |

{% hint style="danger" %}
**`fees`, `valueIn`, and `value` only count transparent value.** For a transaction that moves money in or out of a shielded pool they are wrong, and `fees` is usually reported as `0`.

Transaction `076a0119…` below moves value out of the Sapling pool to one transparent output. The indexer reports:

```json
{
    "vin": [],
    "value": "1503454842",
    "valueIn": "0",
    "fees": "0"
}
```

The real fee is **15,000 zatoshis**. The Sapling pool released 1,503,469,842 zatoshis (`valueBalanceZat` in [api/v2/tx-specific](api-v2-tx-specific-zcash.md)) and the transparent output received 1,503,454,842; the difference is the fee. Compute a shielded transaction's fee as transparent inputs, plus the value balance of each shielded pool, minus transparent outputs.
{% endhint %}

## Use Cases

* **Payment Verification**: Confirm the amount and destination of a transparent deposit
* **Confirmation Checks**: Read the current depth of a transaction
* **Input Tracing**: Follow the transparent addresses a transaction spends

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
