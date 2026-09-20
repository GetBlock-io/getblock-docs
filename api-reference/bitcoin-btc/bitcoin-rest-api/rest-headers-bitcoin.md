---
description: >-
  Example code for the rest/headers REST endpoint. Complete guide on how to use the
  rest/headers REST endpoint in the GetBlock Web3 documentation.
---

# rest/headers - Bitcoin

This endpoint returns a run of block headers, starting at the given block and walking forward along the main chain. Headers carry no transactions, which makes this the cheapest way to follow the chain.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| count | integer | path | Yes | Maximum number of headers to return, starting at the given block |
| hash | string | path | Yes | Hash of the first block in the run |
| format | string | path | Yes | Response format appended to the hash: `json`, `hex`, or `bin` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/headers/3/0000000000000000000159d94328a0362aba4418de918cba6830e550f8e50bd8.json'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/headers/3/0000000000000000000159d94328a0362aba4418de918cba6830e550f8e50bd8.json'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/headers/3/0000000000000000000159d94328a0362aba4418de918cba6830e550f8e50bd8.json')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
[
    {
        "hash": "0000000000000000000159d94328a0362aba4418de918cba6830e550f8e50bd8",
        "confirmations": 2,
        "height": 967815,
        "version": 673554432,
        "versionHex": "2825a000",
        "merkleroot": "4612f8a6b32041ac0b96734a0d04cbeaa887626e7b86b16bf4aa21c4b13bf8e2",
        "time": 1789897746,
        "mediantime": 1789892647,
        "nonce": 1600482865,
        "bits": "17021ec5",
        "target": "000000000000000000021ec50000000000000000000000000000000000000000",
        "difficulty": 132757073449487.5,
        "chainwork": "000000000000000000000000000000000000000148e3a9f92772ad6bab940cd0",
        "nTx": 4162,
        "previousblockhash": "0000000000000000000111b4d4c4fd8b3913403adf3ae8a3ae0932452918fd4e",
        "nextblockhash": "000000000000000000019c481c042c0625abe1dc04fe26795e2e24b152791e5f"
    },
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
        "previousblockhash": "0000000000000000000159d94328a0362aba4418de918cba6830e550f8e50bd8"
    }
]
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| hash | string | The block hash |
| confirmations | numeric | Depth of the block |
| height | numeric | Block height |
| version | numeric | Block version |
| merkleroot | string | Merkle root of the block's transactions |
| time | numeric | Block time as a Unix timestamp |
| mediantime | numeric | Median time of the preceding 11 blocks |
| nonce | numeric | Proof-of-work nonce |
| bits | string | Compact difficulty target |
| difficulty | numeric | Difficulty as a multiple of the minimum |
| chainwork | string | Cumulative work up to this block |
| nTx | numeric | Number of transactions in the block |
| previousblockhash | string | Hash of the preceding block |

{% hint style="info" %}
`count` is a **maximum**, not a guarantee. The run stops at the chain tip: the request above asks for three headers from height 967,815 and returns two, because the tip was 967,816.

The run walks **forward** from the block given, so pass the oldest block you want, not the newest.
{% endhint %}

## Use Cases

* **Light Sync**: Follow the chain by headers without downloading blocks
* **Reorg Detection**: Compare a stored header run against the node's current view
* **Timestamp Series**: Build a height-to-time mapping cheaply

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The path is malformed, or a hash or height is invalid |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No object matches the requested hash, height, or outpoint |
| 500 | Internal error | The node failed to process the request |
