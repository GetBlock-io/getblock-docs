---
description: >-
  Example code for the api/v2/block REST method. Complete guide on how to use
  the api/v2/block REST method in the GetBlock Web3 documentation.
---

# api/v2/block - Dash

This endpoint returns a block by height or hash, including its metadata and a paged list of the transactions it contains.

## Parameters

| Parameter | Type    | Location | Required | Description                                                |
| --------- | ------- | -------- | -------- | ---------------------------------------------------------- |
| blockHash | string  | path     | Yes      | Block height or block hash                                 |
| page      | integer | query    | No       | 1-based page index for the block's transactions. Default 1 |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block/00000000000000007409ab5be18f66b1f68795ff81d3b3b1d592574fae07c372?page=1'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block/00000000000000007409ab5be18f66b1f68795ff81d3b3b1d592574fae07c372?page=1'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block/00000000000000007409ab5be18f66b1f68795ff81d3b3b1d592574fae07c372?page=1')

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
    "hash": "00000000000000007409ab5be18f66b1f68795ff81d3b3b1d592574fae07c372",
    "previousBlockHash": "000000000000000e4dde1797932f95dedc2e816814198d25463626b63032c924",
    "nextBlockHash": "00000000000000078c1330105f565c0eca388b27773b2f11ed6b820b8c82b9be",
    "height": 2540600,
    "confirmations": 74,
    "size": 1703,
    "time": 1789687365,
    "version": 536870912,
    "merkleRoot": "229a9fb3df52a6aba325d36487ac4753124c1cbf6e84d8077466ddc2dfeea7ba",
    "nonce": "2569085062",
    "bits": "191b57db",
    "difficulty": "157073936.0931894",
    "txCount": 2,
    "txs": [
        {
            "txid": "4c14fb8af18ee8370f8d5e1527d9314d323051b579e1acdadd54e6541cf19155",
            "blockHeight": 2540600,
            "confirmations": 74,
            "blockTime": 1789687365,
            "value": "164378041",
            "valueIn": "0",
            "fees": "0"
        }
    ]
}
```

## Response Parameters

| Field             | Type    | Description                         |
| ----------------- | ------- | ----------------------------------- |
| hash              | string  | Block hash                          |
| height            | integer | Block height                        |
| confirmations     | integer | Number of confirmations             |
| time              | integer | Block time as a Unix timestamp      |
| txCount           | integer | Number of transactions in the block |
| txs               | array   | Paged transactions in the block     |
| previousBlockHash | string  | Hash of the parent block            |

## Use Cases

* **Block Explorers**: Render block metadata and its transaction list
* **Chain Indexing**: Page through block transactions into an off-chain store
* **Confirmation Context**: Read block height and time for a transaction's block
* **Throughput Analysis**: Read transaction counts per block over a range

## Error Handling

| HTTP Status | Message        | Description                                   |
| ----------- | -------------- | --------------------------------------------- |
| 400         | Bad request    | The block height or hash is malformed         |
| 404         | Not found      | No block matches the requested height or hash |
| 500         | Internal error | The indexer failed to read the block          |
