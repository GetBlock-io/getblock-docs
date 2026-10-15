---
description: >-
  Get one of your GetBlock Dedicated Nodes, including the APIs and add-ons its
  tokens can use. Complete guide to GET /dedicated/{id} in the Platform API.
---

# Get Dedicated Node - Platform API

`GET /dedicated/{id}` returns one of your Dedicated Nodes, including the `apis` and `addons` a token bound to it may use.

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/dedicated/{id}
```
{% endcode %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Path parameter. The node's `id`. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl https://public-api.getblock.io/api/v1/dedicated/abc123 \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const nodeId = 'abc123';

const response = await fetch(`https://public-api.getblock.io/api/v1/dedicated/${nodeId}`, {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const body = await response.json();

console.log(body.apis);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

node_id = "abc123"

body = requests.get(
    f"https://public-api.getblock.io/api/v1/dedicated/{node_id}",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
).json()

print(body["apis"])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
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
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | The node id, used in the `/dedicated/{id}` routes. |
| `name` | string | The node's name. |
| `status` | string | `created`, `deploying`, `active`, or `disabled`. |
| `protocol` | string | Protocol id. |
| `network` | string | Network id. |
| `sync_mode` | string | `full` or `archive`. |
| `client` | string | The node's client software. |
| `region` | string | Region the node runs in. |
| `performance_grade` | string | `standard` or `high`. |
| `apis` | array of strings | API types the node serves: the valid values of `api` for its tokens. |
| `addons` | array of strings | Add-ons the node serves: the valid values of `addon` for its tokens. |
| `created_at`, `updated_at` | string | RFC 3339 UTC times. |

## Use Cases

* Read the valid `api` and `addon` values before [creating a token](create-dedicated-node-token.md) on the node
* Check a node's client, sync mode, and performance tier

## Error Handling

| HTTP | Cause |
| --- | --- |
| 401 | Missing or invalid API key. |
| 404 | No such node among your Dedicated Nodes. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
