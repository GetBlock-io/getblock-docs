---
description: >-
  Example code for the rest/mempool/info REST endpoint. Complete guide on how to use the
  rest/mempool/info REST endpoint in the GetBlock Web3 documentation.
---

# rest/mempool/info - Bitcoin

This endpoint returns aggregate statistics for the node's memory pool: how many transactions are queued, how much memory they occupy, and the current fee floors.

## Parameters

This endpoint takes no parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/mempool/info.json'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/mempool/info.json'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/mempool/info.json')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "loaded": true,
    "size": 21636,
    "bytes": 5260824,
    "usage": 36534496,
    "total_fee": 0.02524621,
    "maxmempool": 300000000,
    "mempoolminfee": 1e-06,
    "minrelaytxfee": 1e-06,
    "incrementalrelayfee": 1e-06,
    "unbroadcastcount": 0,
    "fullrbf": true,
    "permitbaremultisig": true,
    "maxdatacarriersize": 100000,
    "limitclustercount": 64,
    "limitclustersize": 101000,
    "optimal": true
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| loaded | boolean | True once the mempool has finished loading |
| size | numeric | Number of transactions in the mempool |
| bytes | numeric | Total virtual size of mempool transactions |
| usage | numeric | Memory the mempool occupies, in bytes |
| total_fee | numeric | Sum of all mempool fees, in BTC |
| maxmempool | numeric | Maximum memory the mempool may use, in bytes |
| mempoolminfee | numeric | Minimum fee rate for acceptance, in BTC per kvB. Rises above `minrelaytxfee` when the mempool is full |
| minrelaytxfee | numeric | Minimum relay fee rate, in BTC per kvB |
| incrementalrelayfee | numeric | Minimum fee-rate increment for replacing a transaction |
| unbroadcastcount | numeric | Transactions the node has not yet had confirmed as broadcast |
| fullrbf | boolean | True when the node replaces transactions regardless of opt-in signalling |

{% hint style="info" %}
`mempoolminfee` is the value that matters when broadcasting. It equals `minrelaytxfee` on an uncongested node, but rises once the mempool fills and starts evicting, and a transaction below it is rejected.

Fee rates here are in **BTC per kvB**, not satoshis per vbyte. Multiply by 100,000 to convert: the `1e-06` above is 1 sat/vB.
{% endhint %}

## Use Cases

* **Fee Floors**: Read `mempoolminfee` before broadcasting, so a transaction is not rejected
* **Congestion Checks**: Compare `usage` against `maxmempool` to see how full the node is
* **RBF Logic**: Read `incrementalrelayfee` when sizing a replacement transaction
* **Monitoring**: Track mempool size and total fees over time

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The path is malformed, or a hash or height is invalid |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No object matches the requested hash, height, or outpoint |
| 500 | Internal error | The node failed to process the request |
