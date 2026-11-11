---
description: >-
  Example code for the rest/mempool/contents REST endpoint. Complete guide on how to use the
  rest/mempool/contents REST endpoint in the GetBlock Web3 documentation.
---

# rest/mempool/contents - Bitcoin

This endpoint returns every transaction in the node's memory pool, keyed by transaction id, with fee, size, and ancestry detail for each.

## Parameters

This endpoint takes no parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/mempool/contents.json'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/mempool/contents.json'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/mempool/contents.json')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "ceb5a478b0a44835626fa450b345da2ea238023429da02069d001332b713c720": {
        "vsize": 141,
        "weight": 561,
        "time": 1789899404,
        "height": 967816,
        "descendantcount": 1,
        "descendantsize": 141,
        "ancestorcount": 1,
        "ancestorsize": 141,
        "wtxid": "73f3430babb5c56e0d7aebf85e72a18118305b89ae22e94cee5f62d5618b601b",
        "chunkweight": 561,
        "fees": {
            "base": 0.0001974,
            "modified": 0.0001974,
            "ancestor": 0.0001974,
            "descendant": 0.0001974,
            "chunk": 0.0001974
        },
        "depends": [],
        "spentby": [],
        "bip125-replaceable": true,
        "unbroadcast": false
    }
}
```

The response is an object keyed by txid; one entry is shown.

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| vsize | numeric | Virtual size in vbytes |
| weight | numeric | Transaction weight |
| time | numeric | Unix time the transaction entered the mempool |
| height | numeric | Chain height when the transaction entered the mempool |
| descendantcount | numeric | In-mempool descendants, including this transaction |
| ancestorcount | numeric | In-mempool ancestors, including this transaction |
| wtxid | string | Witness transaction id |
| fees | object | Fees in BTC: `base`, `modified`, and the `ancestor`, `descendant`, and `chunk` aggregates |
| depends | array | Transaction ids of unconfirmed parents |
| spentby | array | Transaction ids of unconfirmed children |
| bip125-replaceable | boolean | True when the transaction may be replaced under RBF |

{% hint style="danger" %}
**This response is very large.** The mempool held 22,536 transactions when the example was captured, and the full response measured **12.7 MB**. Requesting it on a timer will dominate your bandwidth and the node's response time.

Use [rest/mempool/info](rest-mempool-info-bitcoin.md) when you only need counts and fee floors, and request contents sparingly.
{% endhint %}

## Use Cases

* **Fee Estimation**: Build your own estimate from the fee rates currently queued
* **Pending Detection**: Find whether a specific transaction is still waiting
* **Ancestry Analysis**: Read `depends` and `spentby` to map unconfirmed chains

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The path is malformed, or a hash or height is invalid |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No object matches the requested hash, height, or outpoint |
| 500 | Internal error | The node failed to process the request |
