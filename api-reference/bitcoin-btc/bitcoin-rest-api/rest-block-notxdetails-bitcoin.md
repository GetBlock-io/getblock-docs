---
description: >-
  Example code for the rest/block/notxdetails REST endpoint. Complete guide on how to use the
  rest/block/notxdetails REST endpoint in the GetBlock Web3 documentation.
---

# rest/block/notxdetails - Bitcoin

This endpoint returns a block's header fields and the list of transaction ids it contains, without decoding each transaction. It is the practical way to read a block: the full form is many times larger.

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
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/block/notxdetails/000000000000000000019c481c042c0625abe1dc04fe26795e2e24b152791e5f.json'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/block/notxdetails/000000000000000000019c481c042c0625abe1dc04fe26795e2e24b152791e5f.json'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/block/notxdetails/000000000000000000019c481c042c0625abe1dc04fe26795e2e24b152791e5f.json')

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
    "versionHex": "24c74000",
    "merkleroot": "7b7ffdc51c5adbc40f8c209747b7c82f083f647e555cd0ef80b8acf66df21ebf",
    "time": 1789899076,
    "mediantime": 1789892696,
    "nonce": 1539191330,
    "bits": "17021ec5",
    "target": "000000000000000000021ec50000000000000000000000000000000000000000",
    "difficulty": 132757073449487.5,
    "chainwork": "000000000000000000000000000000000000000148e422b78a65626de4ddb156",
    "nTx": 4131,
    "previousblockhash": "0000000000000000000159d94328a0362aba4418de918cba6830e550f8e50bd8",
    "strippedsize": 800904,
    "size": 1590347,
    "weight": 3993059,
    "tx": [
        "8ba102f12c9bb691f3f553fb958c0c209417b43318318e83940372e37ac245f9",
        "98bde4a7f407769028cbe725695d388324ab6b83fab4f310005082dd9b8b2de9"
    ]
}
```

The `tx` array is truncated to two entries; this block holds 4,131 transactions.

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
| tx | array | Transaction ids in the block |

{% hint style="info" %}
Prefer this over [rest/block](rest-block-bitcoin.md) unless every transaction is genuinely needed. The block above is 1.59 MB serialized, and its fully decoded form is several times that, while this response is a header plus a list of ids.
{% endhint %}

## Use Cases

* **Block Scanning**: Walk a block's transaction ids and fetch only the ones you need
* **Chain Walking**: Follow `previousblockhash` to traverse the chain
* **Header Checks**: Read difficulty, timestamps, and work without the payload cost
* **Confirmation Depth**: Read `confirmations` for a known block

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The path is malformed, or a hash or height is invalid |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No object matches the requested hash, height, or outpoint |
| 500 | Internal error | The node failed to process the request |
