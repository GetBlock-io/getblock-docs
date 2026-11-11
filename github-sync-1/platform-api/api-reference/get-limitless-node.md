---
description: >-
  Get one of your GetBlock Limitless Nodes by id. Complete guide to GET
  /limitless/{id} in the Platform API.
---

# Get Limitless Node - Platform API

`GET /limitless/{id}` returns one of your Limitless Nodes. The response has the same shape as one element of [`GET /limitless`](list-limitless-nodes.md).

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/limitless/{id}
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
curl https://public-api.getblock.io/api/v1/limitless/44a33415-21f6-45a9-b529-91e6503f6c1b \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const nodeId = '44a33415-21f6-45a9-b529-91e6503f6c1b';

const response = await fetch(`https://public-api.getblock.io/api/v1/limitless/${nodeId}`, {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const body = await response.json();

console.log(body.rps_limit);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

node_id = "44a33415-21f6-45a9-b529-91e6503f6c1b"

body = requests.get(
    f"https://public-api.getblock.io/api/v1/limitless/{node_id}",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
).json()

print(body["rps_limit"])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
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
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | The node id, used in the `/limitless/{id}` routes. |
| `status` | string | The node subscription's status, such as `active` or `pending`. |
| `protocol` | string | Protocol id. |
| `network` | string | Network id. |
| `region` | string | Region serving the node. |
| `mode` | string | Sync mode. |
| `rps_limit` | integer | Guaranteed requests per second. |
| `grade` | string | The node's tier. |
| `apis` | array of strings | API types your existing tokens on the node serve. Any API in the catalog for the node's protocol may still be requested. |

## Use Cases

* Check a node's status after ordering it
* Read the node's protocol and mode before creating a token on it

## Error Handling

| HTTP | Cause |
| --- | --- |
| 401 | Missing or invalid API key. |
| 404 | No such node among your Limitless Nodes. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
