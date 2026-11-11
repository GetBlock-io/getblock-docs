---
description: >-
  List your GetBlock access tokens with filters by protocol, network, region,
  and name. Complete guide to GET /tokens in the Platform API.
---

# List access tokens - Platform API

`GET /tokens` returns your access tokens, most recently updated first. Only usable tokens are listed: deleted tokens and tokens replaced by a rotation are left out.

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/tokens
```
{% endcode %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `limit` | integer | No | Query parameter. Page size. Default 20, at most 100. |
| `offset` | integer | No | Query parameter. Items to skip. Default 0. |
| `name` | string | No | Query parameter. Substring of the token name, such as `eth-9ee4d`. |
| `protocol` | string | No | Query parameter. Protocol id, such as `eth`. |
| `network` | string | No | Query parameter. Network id, such as `mainnet`. |
| `region` | string | No | Query parameter. Region, such as `eu-central-1`. |
| `namespace` | string | No | Query parameter. Access type, such as `shared`. |

Filters combine with AND and are case-insensitive. See [Pagination](objects-and-conventions.md#pagination).

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl 'https://public-api.getblock.io/api/v1/tokens?protocol=eth' \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const response = await fetch('https://public-api.getblock.io/api/v1/tokens?protocol=eth', {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const body = await response.json();

console.log(body.tokens.map((t) => t.endpoint));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

body = requests.get(
    "https://public-api.getblock.io/api/v1/tokens",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    params={"protocol": "eth"},
).json()

print([t["endpoint"] for t in body["tokens"]])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "total": 2,
  "limit": 20,
  "offset": 0,
  "tokens": [
    {
      "id": "9b2796d7b4c448de83b01644bb93f625",
      "name": "eth-9b279",
      "status": "active",
      "namespace": "shared",
      "protocol": "eth",
      "network": "mainnet",
      "api": "json-rpc",
      "mode": "archive",
      "region": "eu-central-1",
      "addon": "",
      "dedicated_node_id": "",
      "limitless_node_id": "",
      "endpoint": "https://shared.eu-central-1.getblock.io/9b2796d7b4c448de83b01644bb93f625/",
      "created_at": "2026-10-02T13:19:13Z",
      "updated_at": "2026-10-02T13:19:13Z"
    },
    {
      "id": "e41a7c09d2b54f6e8a3c1b07f9d26e58",
      "name": "eth-e41a7",
      "status": "active",
      "namespace": "shared",
      "protocol": "eth",
      "network": "sepolia",
      "api": "json-rpc",
      "mode": "full",
      "region": "eu-central-1",
      "addon": "",
      "dedicated_node_id": "",
      "limitless_node_id": "",
      "endpoint": "https://shared.eu-central-1.getblock.io/e41a7c09d2b54f6e8a3c1b07f9d26e58/",
      "created_at": "2026-09-14T08:02:51Z",
      "updated_at": "2026-09-14T08:02:51Z"
    }
  ]
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `total` | integer | Tokens matching the filters, before `limit` and `offset`. |
| `limit` | integer | Page size applied. |
| `offset` | integer | Items skipped. |
| `tokens` | array of objects | The tokens on this page. See [The access token object](objects-and-conventions.md#the-access-token-object). |

## Use Cases

* Find every endpoint for one chain before migrating it
* Audit which tokens exist and which are `locked`
* Count tokens against the plan's `max_tokens` from [`GET /subscription`](get-subscription.md)

## Error Handling

| HTTP | Cause |
| --- | --- |
| 401 | Missing or invalid API key. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
