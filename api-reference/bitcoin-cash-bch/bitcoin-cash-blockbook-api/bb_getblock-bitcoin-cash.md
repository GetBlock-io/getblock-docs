---
description: >-
  Example code for the bb_getBlock JSON-RPC method. Complete guide on how to use
  the bb_getBlock JSON-RPC method in the GetBlock Web3 documentation.
---

# bb\_getBlock - Bitcoin Cash

This method returns a block by height or hash, including its metadata and a paged list of the transactions it contains.

## Parameters

| Parameter | Type    | Required | Description                                                |
| --------- | ------- | -------- | ---------------------------------------------------------- |
| blockId   | string  | Yes      | Block height or block hash                                 |
| page      | integer | No       | 1-based page index for the block's transactions. Default 1 |

## Request

{% tabs %}
{% tab title="cURL (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/api/v2/block/0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc?page=1'
```
{% endcode %}
{% endtab %}

{% tab title="cURL (JSON-RPC)" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "bb_getBlock",
    "params": [
        "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc",
        {
            "page": 1
        }
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/api/v2/block/0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc?page=1'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/api/v2/block/0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc?page=1')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "page": 1,
    "totalPages": 3,
    "itemsOnPage": 1000,
    "hash": "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc",
    "previousBlockHash": "000000000000000001d3a318372df5d1eec54462a0d7471ae1cdf49838f793dd",
    "nextBlockHash": "000000000000000000006d8e1eb870bd281b30ed621acf6b8d6af2a3c7ab61f1",
    "height": 684634,
    "confirmations": 1197,
    "size": 1350854,
    "time": 1617180599,
    "version": 1073733632,
    "merkleRoot": "d14c9f467c4bdd5135837696150ab5f52f3f5043de324ca4e5766b195b9f8f37",
    "nonce": "3669423616",
    "bits": "170cdf6f",
    "difficulty": "21865558044610.55",
    "txCount": 2815,
    "txs": [
        {
            "txid": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642",
            "value": "9407625",
            "fees": "2938345"
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

| Error Code   | Message        | Description                                   |
| ------------ | -------------- | --------------------------------------------- |
| 400 / -32602 | Bad request    | The block height or hash is malformed         |
| 404 / -32603 | Not found      | No block matches the requested height or hash |
| 500 / -32603 | Internal error | The indexer failed to read the block          |
