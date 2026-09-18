---
description: >-
  Example code for the api/v2/block REST method. Complete guide on how to use the
  api/v2/block REST method in the GetBlock Web3 documentation.
---

# api/v2/block - Litecoin

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
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block/0fc24234c2379a5262023a4b4a653ff3a11df6bb1c12851aff820d27ea02b0b6?page=1'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block/0fc24234c2379a5262023a4b4a653ff3a11df6bb1c12851aff820d27ea02b0b6?page=1'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block/0fc24234c2379a5262023a4b4a653ff3a11df6bb1c12851aff820d27ea02b0b6?page=1')

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
    "hash": "0fc24234c2379a5262023a4b4a653ff3a11df6bb1c12851aff820d27ea02b0b6",
    "previousBlockHash": "e8a9b97499a1bf0bb21ebef8e30acf8fafb17145b7bd4269d5192e8039993941",
    "nextBlockHash": "eec4238a5bc1d4cbd2f2b3738bf34ca63a22c8db81ed4bf73a12360350ce8ada",
    "height": 3179800,
    "confirmations": 17,
    "size": 90534,
    "time": 1789699786,
    "version": 536870932,
    "merkleRoot": "4f5ddddee343643887ba017db6d76e7b5b834562a9198b60bc99cb84233bd22b",
    "nonce": "1815478550",
    "bits": "19309741",
    "difficulty": "88389131.60278592",
    "txCount": 60,
    "txs": [
        {
            "txid": "0df2b3ad2a6bae31bdf82108c01d7ae27dafe69cadb0188d542df9035982bbc0",
            "blockHeight": 3179800,
            "confirmations": 17,
            "blockTime": 1789699786,
            "value": "625073004",
            "valueIn": "0",
            "fees": "0"
        }
    ]
}
```

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
| merkleRoot | string | Merkle root of the block's transactions |
| difficulty | string | Proof-of-work difficulty at this block |
| txCount | integer | Total number of transactions in the block |
| txs | array | Transactions on the requested page |

{% hint style="info" %}
Height lookups always resolve on the current main chain. A hash lookup can still return a block from another fork if the node retains it. Large blocks page at 1000 transactions, so check `totalPages` before assuming a single request covers the block.
{% endhint %}

## Use Cases

* **Explorers**: Render a block and its transactions
* **Chain Walking**: Follow previousBlockHash and nextBlockHash to traverse the chain
* **Reconciliation**: Scan a block's transactions for addresses of interest
* **Confirmation Context**: Read a block's depth and timestamp

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The block id is not a valid height or hash |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No block matches the requested id |
| 500 | Internal error | The indexer failed to read the block |
