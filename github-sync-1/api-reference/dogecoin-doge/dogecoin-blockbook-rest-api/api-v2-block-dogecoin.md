---
description: >-
  Example code for the api/v2/block REST method. Complete guide on how to use the
  api/v2/block REST method in the GetBlock Web3 documentation.
---

# api/v2/block - Dogecoin

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
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block/afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf?page=1'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block/afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf?page=1'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block/afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf?page=1')

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
    "hash": "afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf",
    "previousBlockHash": "b1b6e89d831683c62b79af87de01df41caa6914fa977e5877027b14d99f0f8f6",
    "nextBlockHash": "2ce761c3dff30841f1435165b3315ddf163243477d7bf1969f84e2608ac3a855",
    "height": 6378930,
    "confirmations": 43,
    "size": 55958,
    "time": 1789703885,
    "version": 6422788,
    "merkleRoot": "3033dfe05f5377ca20680dbba85eefd14adbf932ddece22d06980538e8d14809",
    "nonce": "0",
    "bits": "1967ad65",
    "difficulty": "41425662.4408129",
    "txCount": 70,
    "txs": [
        {
            "txid": "d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed",
            "blockHeight": 6378930,
            "confirmations": 43,
            "blockTime": 1789703885,
            "value": "499999999659500",
            "valueIn": "500000000000000",
            "fees": "340500"
        }
    ]
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| hash | string | Block hash |
| previousBlockHash | string | Hash of the preceding block |
| height | integer | Block height |
| confirmations | integer | Number of confirmations |
| time | integer | Block timestamp |
| size | integer | Block size in bytes |
| merkleRoot | string | Merkle root of the block's transactions |
| nonce | string | Block nonce. Always `0` on Dogecoin, which is merge-mined with Litecoin |
| bits | string | Compact difficulty target |
| difficulty | string | Proof-of-work difficulty at this block |
| txCount | integer | Total number of transactions in the block |
| txs | array | Transactions on the requested page |

{% hint style="info" %}
Dogecoin targets a one-minute block interval, so heights advance about ten times faster than Bitcoin. A confirmation depth copied from a Bitcoin integration settles in roughly a tenth of the wall-clock time; choose depth by the time you need.
{% endhint %}

## Use Cases

* **Explorers**: Render a block and its transactions
* **Chain Walking**: Follow previousBlockHash to traverse the chain
* **Reconciliation**: Scan a block's transactions for addresses of interest

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
