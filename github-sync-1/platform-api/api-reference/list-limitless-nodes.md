---
description: >-
  List your GetBlock Limitless Nodes with their guaranteed RPS, region, and
  status. Complete guide to GET /limitless in the Platform API.
---

# List Limitless Nodes - Platform API

`GET /limitless` returns your [Limitless Nodes](../../getting-started/plans-and-limits/limitless-node.md). Nodes are ordered in the [dashboard](https://account.getblock.io/), not through the API, so there is no create or delete endpoint.

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/limitless
```
{% endcode %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `limit` | integer | No | Query parameter. Page size. Default 20, at most 100. |
| `offset` | integer | No | Query parameter. Items to skip. Default 0. |
| `protocol` | string | No | Query parameter. Protocol id, such as `eth`. |
| `network` | string | No | Query parameter. Network id, such as `mainnet`. |
| `region` | string | No | Query parameter. Region, such as `ap-southeast-1`. |
| `status` | string | No | Query parameter. Such as `active` or `pending`. |

Filters combine with AND and are case-insensitive.

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl 'https://public-api.getblock.io/api/v1/limitless?protocol=eth' \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const response = await fetch('https://public-api.getblock.io/api/v1/limitless?protocol=eth', {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const body = await response.json();

console.log(body.nodes.map((n) => `${n.id} ${n.rps_limit} RPS`));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

body = requests.get(
    "https://public-api.getblock.io/api/v1/limitless",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    params={"protocol": "eth"},
).json()

print([(n["id"], n["rps_limit"]) for n in body["nodes"]])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "total": 1,
  "limit": 20,
  "offset": 0,
  "nodes": [
    {
      "id": "44a33415-21f6-45a9-b529-91e6503f6c1b",
      "status": "active",
      "protocol": "eth",
      "network": "mainnet",
      "region": "ap-southeast-1",
      "mode": "full",
      "rps_limit": 50,
      "grade": "full-growth",
      "apis": ["json-rpc", "ws"]
    }
  ]
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `total` | integer | Nodes matching the filters, before `limit` and `offset`. |
| `limit` | integer | Page size applied. |
| `offset` | integer | Items skipped. |
| `nodes` | array of objects | The nodes on this page. See [The Limitless Node object](objects-and-conventions.md#the-limitless-node-object). |

## Use Cases

* Read each node's guaranteed RPS to size client-side rate limiting
* Find the node to bind a new token to

## Error Handling

| HTTP | Cause |
| --- | --- |
| 401 | Missing or invalid API key. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
