---
description: >-
  Example code for the api/v2/tx REST method. Complete guide on how to use the
  api/v2/tx REST method in the GetBlock Web3 documentation.
---

# api/v2/tx - Litecoin

This endpoint returns a normalized transaction by its id, with inputs, outputs, addresses, values, and confirmation data in the indexer's unified schema. Coin-specific fields that do not fit the shared shape are available from [api/v2/tx-specific](api-v2-tx-specific-litecoin.md).

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
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx/51a31c4236a99bfd17cda606f079c9c4814851119e7593ab7c739bd5972b1969'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx/51a31c4236a99bfd17cda606f079c9c4814851119e7593ab7c739bd5972b1969'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx/51a31c4236a99bfd17cda606f079c9c4814851119e7593ab7c739bd5972b1969')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "txid": "51a31c4236a99bfd17cda606f079c9c4814851119e7593ab7c739bd5972b1969",
    "version": 2,
    "vin": [
        {
            "txid": "4c3d9548bb03a42551c10f2472e99cd86776ed00fd3bfb530c4466b93e7ddac7",
            "vout": 2121,
            "sequence": 4294967295,
            "n": 0,
            "addresses": [
                "LUnYgfpJMLM2h27wDcJypQHQhRXXrZrfvE"
            ],
            "isAddress": true,
            "value": "720950"
        }
    ],
    "vout": [
        {
            "value": "8324080358",
            "n": 0,
            "hex": "76a914182c25bf95cacc04fd35d1ea8ea49539251bd55288ac",
            "addresses": [
                "LMRmNEdhi5F43vtW5qPdv1q9s7xsMp4XK5"
            ],
            "isAddress": true
        }
    ],
    "blockHash": "0fc24234c2379a5262023a4b4a653ff3a11df6bb1c12851aff820d27ea02b0b6",
    "blockHeight": 3179800,
    "confirmations": 17,
    "blockTime": 1789699786,
    "size": 73090,
    "vsize": 73090,
    "value": "103517609950",
    "valueIn": "103517609950",
    "fees": "0"
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| txid | string | The transaction id |
| vin | array | Inputs, with the address and value of each spent output |
| vout | array | Outputs, with value, script, and destination addresses |
| blockHash | string | Hash of the containing block. Absent while unconfirmed |
| blockHeight | integer | Block height, or -1 while unconfirmed |
| confirmations | integer | Number of confirmations. 0 while unconfirmed |
| blockTime | integer | Block timestamp, or first-seen time for a mempool transaction |
| size | integer | Serialized size in bytes |
| vsize | integer | Virtual size in vbytes, which differs from size for SegWit transactions |
| value | string | Total output value in litoshis |
| valueIn | string | Total input value in litoshis |
| fees | string | Fee paid in litoshis |
| rbf | boolean | True when the transaction signals replace-by-fee |

{% hint style="info" %}
Empty fields are omitted rather than returned as null, so `lockTime` and `rbf` are absent unless set. For a mempool transaction, `blockTime` is when this indexer first saw the transaction, not a consensus timestamp.
{% endhint %}

## Use Cases

* **Transaction Views**: Render a normalized transaction in a wallet or explorer
* **Payment Confirmation**: Read confirmations to confirm a received payment
* **Fee Inspection**: Compare fees paid against the transaction's virtual size
* **Address Mapping**: Read input and output addresses without decoding raw hex

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The transaction id is not a valid 64-character hex string |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No transaction with the requested id is indexed |
| 500 | Internal error | The indexer failed to read the transaction |
