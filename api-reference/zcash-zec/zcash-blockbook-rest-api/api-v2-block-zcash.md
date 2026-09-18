---
description: >-
  Example code for the api/v2/block REST method. Complete guide on how to use the
  api/v2/block REST method in the GetBlock Web3 documentation.
---

# api/v2/block - Zcash

This endpoint returns block information with a paged list of the transactions it contains. The block can be selected by height or by hash.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| blockId | string | path | Yes | Block height or block hash |
| page | integer | query | No | 1-based page index over the block's transactions |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block/00000000000f3c0bdf81aba0bf081c5c4882975cbc0a8ce35e70ddb5f265f1b1?page=1'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block/00000000000f3c0bdf81aba0bf081c5c4882975cbc0a8ce35e70ddb5f265f1b1?page=1'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block/00000000000f3c0bdf81aba0bf081c5c4882975cbc0a8ce35e70ddb5f265f1b1?page=1')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "page": 1,
    "totalPages": 1,
    "itemsOnPage": 1000,
    "hash": "00000000000f3c0bdf81aba0bf081c5c4882975cbc0a8ce35e70ddb5f265f1b1",
    "previousBlockHash": "000000000007b84c009ac090a37dd74b23bcfa4aeb6620960c7d0ee3f5fb664a",
    "nextBlockHash": "00000000000aa124983da79c1e3bd8524b5b76da7b43bb1a88edd93ac5f61331",
    "height": 3487790,
    "confirmations": 46,
    "size": 216827,
    "time": 1789741929,
    "version": 4,
    "merkleRoot": "2134e7beb96aac05105adbbe8ef04fab84739f8a90e22e73e44e6add4812ed35",
    "nonce": "a341002a000000000000000000000000000000000000000000000000d094f930",
    "bits": "1b762f01",
    "difficulty": "290731287.69865835",
    "txCount": 32,
    "txs": [
        {
            "txid": "bdfca4416851a17616c5900903ef41907429f62302731db3517e9c5815287b42",
            "vin": [
                {
                    "n": 0,
                    "isAddress": false,
                    "value": "0"
                }
            ],
            "vout": [
                {
                    "value": "125600000",
                    "n": 0,
                    "addresses": [
                        "t1XQZdZMnzXBcL8yx2PR27dSNrqctgwLgux"
                    ],
                    "isAddress": true
                }
            ],
            "blockHeight": 3487790,
            "confirmations": 46,
            "blockTime": 1789741929,
            "value": "138100000",
            "valueIn": "0",
            "fees": "0"
        }
    ]
}
```

The `txs` array is truncated to its first entry, the coinbase transaction, and that transaction's `vout` to one output.

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| hash | string | Block hash |
| previousBlockHash | string | Hash of the preceding block |
| nextBlockHash | string | Hash of the following block, when one exists |
| height | integer | Block height |
| confirmations | integer | Number of confirmations |
| size | integer | Block size in bytes |
| time | integer | Block timestamp |
| nonce | string | The 32-byte Equihash nonce, hex-encoded |
| bits | string | Compact difficulty target |
| difficulty | string | Proof-of-work difficulty |
| txCount | integer | Total number of transactions in the block |
| txs | array | Transactions on the requested page, in the normalized schema |

{% hint style="info" %}
Zcash targets a **75-second** block interval, so heights advance about eight times as fast as Bitcoin's. The `nonce` is a 32-byte value rather than Bitcoin's 4-byte integer, because Zcash mines with Equihash.

Transactions in `txs` use the normalized schema, which counts transparent value only. A shielded transaction's `fees` there read `0`; see [api/v2/tx](api-v2-tx-zcash.md).
{% endhint %}

## Use Cases

* **Explorers**: Render a block and its transactions
* **Chain Walking**: Follow previousBlockHash and nextBlockHash to traverse the chain
* **Reconciliation**: Scan a block's transactions for transparent addresses of interest

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
