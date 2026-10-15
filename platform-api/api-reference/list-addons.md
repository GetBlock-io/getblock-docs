---
description: >-
  List the add-ons available on GetBlock Shared Nodes and the protocols that
  offer them. Complete guide to GET /addons in the Platform API.
---

# List add-ons - Platform API

`GET /addons` lists the [add-ons](../../add-ons/overview.md) that can be attached to a Shared Node token, each once, with the protocols that offer it. Broadly available add-ons come first.

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/addons
```
{% endcode %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `limit` | integer | No | Query parameter. Page size. Default 20, at most 100. |
| `offset` | integer | No | Query parameter. Items to skip. Default 0. |
| `search` | string | No | Query parameter. Case-insensitive substring of the id or the name, such as `mev`. |
| `protocol` | string | No | Query parameter. Only add-ons offered on this protocol, such as `eth`. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl 'https://public-api.getblock.io/api/v1/addons?protocol=eth' \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const response = await fetch('https://public-api.getblock.io/api/v1/addons?protocol=eth', {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const body = await response.json();

console.log(body.addons.map((a) => a.id));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

body = requests.get(
    "https://public-api.getblock.io/api/v1/addons",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    params={"protocol": "eth"},
).json()

print([a["id"] for a in body["addons"]])
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
  "addons": [
    {
      "id": "mevprotected",
      "name": "MEV protected",
      "description": "Sends transactions through a private route to protect them from front-running and sandwich attacks.",
      "documentation_url": "https://docs.getblock.io/add-ons/mev-protection",
      "protocols": ["eth", "bsc", "base", "sol", "unichain"]
    },
    {
      "id": "mevprotectedrelay",
      "name": "MEV protected - Relay",
      "description": "MEV protection through a relay.",
      "documentation_url": "",
      "protocols": ["eth"]
    }
  ]
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `total` | integer | Add-ons matching the filters, before `limit` and `offset`. |
| `limit` | integer | Page size applied. |
| `offset` | integer | Items skipped. |
| `addons[].id` | string | What to send as `addon` when creating a token. |
| `addons[].name` | string | Display name. |
| `addons[].description` | string | What the add-on does. |
| `addons[].documentation_url` | string | Link to the add-on's docs. Empty when it has none. |
| `addons[].protocols` | array of strings | Protocols that offer the add-on. |

Where exactly an add-on is served (network, sync mode, and API type) is in [`GET /protocols/{id}`](get-protocol.md).

## Use Cases

* Find the `addon` id for an MEV-protected endpoint
* Check which chains offer an add-on

## Error Handling

| HTTP | Cause |
| --- | --- |
| 401 | Missing or invalid API key. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
