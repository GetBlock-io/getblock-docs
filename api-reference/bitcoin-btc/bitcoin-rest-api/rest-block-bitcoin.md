---
description: >-
  Example code for the rest/block REST endpoint. Complete guide on how to use the
  rest/block REST endpoint in the GetBlock Web3 documentation.
---

# rest/block - Bitcoin

This endpoint returns a block with every transaction decoded in full. The same block is available as raw hex at `.hex` and as raw binary at `.bin`.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| hash | string | path | Yes | The block hash |
| format | string | path | Yes | Response format appended to the hash: `json`, `hex`, or `bin` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/block/000000000000000000019c481c042c0625abe1dc04fe26795e2e24b152791e5f.json'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/block/000000000000000000019c481c042c0625abe1dc04fe26795e2e24b152791e5f.json'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/block/000000000000000000019c481c042c0625abe1dc04fe26795e2e24b152791e5f.json')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "hash": "000000000000000000019c481c042c0625abe1dc04fe26795e2e24b152791e5f",
    "confirmations": 1,
    "height": 967816,
    "version": 617037824,
    "merkleroot": "7b7ffdc51c5adbc40f8c209747b7c82f083f647e555cd0ef80b8acf66df21ebf",
    "time": 1789899076,
    "nonce": 1539191330,
    "bits": "17021ec5",
    "difficulty": 132757073449487.5,
    "nTx": 4131,
    "previousblockhash": "0000000000000000000159d94328a0362aba4418de918cba6830e550f8e50bd8",
    "strippedsize": 800904,
    "size": 1590347,
    "weight": 3993059,
    "tx": [
        {
            "txid": "98bde4a7f407769028cbe725695d388324ab6b83fab4f310005082dd9b8b2de9",
            "hash": "290ba098c730ca593384647c6c55f4233a60fbdaa9055fd1fe54f51f49a30ad0",
            "version": 2,
            "size": 205,
            "vsize": 154,
            "weight": 616,
            "locktime": 0,
            "vin": [
                "..."
            ],
            "vout": [
                "..."
            ],
            "hex": "02000000000101..."
        }
    ]
}
```

The `tx` array is truncated to one transaction, with its inputs and outputs elided; this block holds 4,131.

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| hash | string | The block hash |
| confirmations | numeric | Depth of the block, or -1 if not on the main chain |
| height | numeric | Block height |
| version | numeric | Block version |
| versionHex | string | Block version in hex |
| merkleroot | string | Merkle root of the block's transactions |
| time | numeric | Block time as a Unix timestamp |
| mediantime | numeric | Median time of the preceding 11 blocks |
| nonce | numeric | Proof-of-work nonce |
| bits | string | Compact difficulty target |
| target | string | Full proof-of-work target |
| difficulty | numeric | Difficulty as a multiple of the minimum |
| chainwork | string | Cumulative work up to this block |
| nTx | numeric | Number of transactions in the block |
| previousblockhash | string | Hash of the preceding block |
| nextblockhash | string | Hash of the following block, when one exists |
| strippedsize | numeric | Block size excluding witness data |
| size | numeric | Block size in bytes |
| weight | numeric | Block weight |
| tx | array | Fully decoded transactions, each in the same shape as [rest/tx](rest-tx-bitcoin.md) |

{% hint style="warning" %}
Responses are large. This block is 1.59 MB serialized and decodes to a far bigger JSON document, and a request for it transfers the whole thing. For most work [rest/block/notxdetails](rest-block-notxdetails-bitcoin.md) is the better call, followed by [rest/tx](rest-tx-bitcoin.md) for the transactions you actually want.

As with `rest/tx`, values are in BTC as decimals and no per-transaction fee is included.
{% endhint %}

## Use Cases

* **Full Block Analysis**: Read every transaction in a block in one request
* **Indexing**: Build a local index from decoded block contents
* **Raw Retrieval**: Fetch the serialized block with the `.hex` or `.bin` form

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The path is malformed, or a hash or height is invalid |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No object matches the requested hash, height, or outpoint |
| 500 | Internal error | The node failed to process the request |
