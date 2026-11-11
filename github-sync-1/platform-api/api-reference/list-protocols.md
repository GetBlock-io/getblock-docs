---
description: >-
  List the blockchains available on GetBlock Shared Nodes. Complete guide to
  GET /protocols in the Platform API.
---

# List protocols - Platform API

`GET /protocols` lists the protocols served on Shared Nodes, most prominent first. Every protocol listed can have a token created for it. Each entry is an identity only; its networks, modes, API types, and regions are in [`GET /protocols/{id}`](get-protocol.md).

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/protocols
```
{% endcode %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `limit` | integer | No | Query parameter. Page size. Default 20, at most 100. |
| `offset` | integer | No | Query parameter. Items to skip. Default 0. |
| `search` | string | No | Query parameter. Case-insensitive substring of the id or the name: `eth` and `ether` both find Ethereum. Compatibility is not searched: `eth` does not return every EVM chain. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl 'https://public-api.getblock.io/api/v1/protocols?search=eth' \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const response = await fetch('https://public-api.getblock.io/api/v1/protocols?search=eth', {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const body = await response.json();

console.log(body.protocols.map((p) => p.id));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

body = requests.get(
    "https://public-api.getblock.io/api/v1/protocols",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    params={"search": "eth"},
).json()

print([p["id"] for p in body["protocols"]])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "total": 3,
  "limit": 20,
  "offset": 0,
  "protocols": [
    {
      "id": "eth",
      "name": "Ethereum",
      "based_on": ""
    },
    {
      "id": "etc",
      "name": "Ethereum Classic",
      "based_on": "eth"
    },
    {
      "id": "etherlink",
      "name": "Etherlink",
      "based_on": "eth"
    }
  ]
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `total` | integer | Protocols matching the filters, before `limit` and `offset`. |
| `limit` | integer | Page size applied. |
| `offset` | integer | Items skipped. |
| `protocols[].id` | string | What to send as `protocol` when creating a token. |
| `protocols[].name` | string | Display name. |
| `protocols[].based_on` | string | The protocol this one is compatible with, such as `eth` for EVM chains. Empty when it stands on its own. |

This is the Shared Node catalog. A token on a Dedicated or Limitless Node takes its protocol and network from the node.

## Use Cases

* Find the protocol id to send to [`POST /tokens`](create-token.md)
* Build a chain picker in an internal tool

## Error Handling

| HTTP | Cause |
| --- | --- |
| 401 | Missing or invalid API key. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
