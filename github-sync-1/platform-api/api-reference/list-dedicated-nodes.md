---
description: >-
  List your GetBlock Dedicated Nodes with their protocol, network, region, and
  status. Complete guide to GET /dedicated in the Platform API.
---

# List Dedicated Nodes - Platform API

`GET /dedicated` returns your Dedicated Nodes. Nodes are ordered in the [dashboard](https://account.getblock.io/), not through the API, so there is no create or delete endpoint.

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/dedicated
```
{% endcode %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `limit` | integer | No | Query parameter. Page size. Default 20, at most 100. |
| `offset` | integer | No | Query parameter. Items to skip. Default 0. |
| `protocol` | string | No | Query parameter. Protocol id, such as `eth`. |
| `network` | string | No | Query parameter. Network id, such as `mainnet`. |
| `region` | string | No | Query parameter. Region, such as `eu-central-1`. |
| `status` | string | No | Query parameter. `created`, `deploying`, `active`, or `disabled`. |

Filters combine with AND and are case-insensitive.

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl 'https://public-api.getblock.io/api/v1/dedicated?status=active' \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const response = await fetch('https://public-api.getblock.io/api/v1/dedicated?status=active', {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const body = await response.json();

console.log(body.nodes.map((n) => `${n.id} ${n.status}`));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

body = requests.get(
    "https://public-api.getblock.io/api/v1/dedicated",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    params={"status": "active"},
).json()

print([(n["id"], n["status"]) for n in body["nodes"]])
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
      "id": "abc123",
      "name": "eth-dedicated-1",
      "status": "active",
      "protocol": "eth",
      "network": "mainnet",
      "sync_mode": "archive",
      "client": "geth",
      "region": "eu-central-1",
      "performance_grade": "high",
      "apis": ["jsonrpc", "ws"],
      "addons": ["debug"],
      "created_at": "2026-01-02T03:04:05Z",
      "updated_at": "2026-02-03T04:05:06Z"
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
| `nodes` | array of objects | The nodes on this page. See [The Dedicated Node object](objects-and-conventions.md#the-dedicated-node-object). |

## Use Cases

* Wait for a newly ordered node to reach `active` before creating tokens on it
* Inventory nodes by region or protocol

## Error Handling

| HTTP | Cause |
| --- | --- |
| 401 | Missing or invalid API key. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
