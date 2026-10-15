---
description: >-
  Get one protocol's networks, sync modes, API types, regions, and add-ons:
  every valid token configuration. Complete guide to GET /protocols/{id} in
  the Platform API.
---

# Get protocol - Platform API

`GET /protocols/{id}` returns everything needed to build a valid [create-token](create-token.md) request for one protocol: its networks, the sync modes per network, the API types per mode, and the regions per API type.

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/protocols/{id}
```
{% endcode %}

{% hint style="info" %}
**Read the response downwards.** Pick a network, then a mode it offers, then an API in that mode, then a region listed for that API. A combination assembled any other way may not exist.
{% endhint %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Path parameter. Protocol id, such as `eth`. Case-insensitive. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl https://public-api.getblock.io/api/v1/protocols/eth \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const protocolId = 'eth';

const response = await fetch(`https://public-api.getblock.io/api/v1/protocols/${protocolId}`, {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const body = await response.json();

console.log(body.networks.map((n) => n.id));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

protocol_id = "eth"

body = requests.get(
    f"https://public-api.getblock.io/api/v1/protocols/{protocol_id}",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
).json()

print([n["id"] for n in body["networks"]])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "id": "eth",
  "name": "Ethereum",
  "based_on": "",
  "networks": [
    {
      "id": "mainnet",
      "name": "Mainnet",
      "paid_regions": [],
      "modes": [
        {
          "id": "full",
          "apis": [
            {
              "id": "json-rpc",
              "name": "JSON-RPC",
              "regions": ["eu-central-1", "us-east-1"]
            },
            {
              "id": "ws",
              "name": "WebSocket",
              "regions": ["eu-central-1", "us-east-1"]
            },
            {
              "id": "graphql",
              "name": "GraphQL",
              "regions": ["eu-central-1"]
            }
          ],
          "addons": [
            {
              "id": "mevprotected",
              "name": "MEV protected",
              "apis": [
                {
                  "id": "json-rpc",
                  "name": "JSON-RPC",
                  "regions": ["eu-central-1", "us-east-1"]
                }
              ]
            }
          ]
        },
        {
          "id": "archive",
          "apis": [
            {
              "id": "json-rpc",
              "name": "JSON-RPC",
              "regions": ["eu-central-1", "us-east-1"]
            }
          ],
          "addons": []
        }
      ]
    },
    {
      "id": "sepolia",
      "name": "Sepolia",
      "paid_regions": [],
      "modes": [
        {
          "id": "full",
          "apis": [
            {
              "id": "json-rpc",
              "name": "JSON-RPC",
              "regions": ["eu-central-1"]
            }
          ],
          "addons": []
        }
      ]
    }
  ]
}
```
{% endcode %}

Response shortened. The live catalog also lists other networks, such as Hoodi.

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | Protocol id. |
| `name` | string | Display name. |
| `based_on` | string | The protocol this one is compatible with. Empty when it stands on its own. |
| `networks[].id` | string | What to send as `network`. |
| `networks[].name` | string | Display name. |
| `networks[].paid_regions` | array of strings | Regions that need a paid plan. Creating a token in one on a free plan is refused with `403`. |
| `networks[].modes[].id` | string | What to send as `mode`: `full` or `archive`. |
| `networks[].modes[].apis[]` | array of objects | API types the mode serves. `id` is what to send as `api`; `regions` lists the only regions that serve it. |
| `networks[].modes[].addons[]` | array of objects | Add-ons available in the mode. `id` is what to send as `addon`; `apis` lists the API types the add-on is served on, which may be fewer than the mode's own. |

Add-on descriptions and documentation links are in [`GET /addons`](list-addons.md).

## Use Cases

* Validate a token configuration before sending it
* Let an AI agent pick a valid network, mode, API, and region
* Check whether a region needs a paid plan

## Error Handling

| HTTP | Cause |
| --- | --- |
| 401 | Missing or invalid API key. |
| 404 | No such protocol in the catalog. A protocol with nothing currently available answers the same way. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
