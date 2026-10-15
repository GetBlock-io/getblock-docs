---
description: >-
  Get one of your GetBlock access tokens by id. Complete guide to GET
  /tokens/{id} in the Platform API.
---

# Get access token - Platform API

`GET /tokens/{id}` returns one of your access tokens. The response has the same shape as one element of [`GET /tokens`](list-tokens.md).

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/tokens/{id}
```
{% endcode %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Path parameter. The token's `id`. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl https://public-api.getblock.io/api/v1/tokens/9b2796d7b4c448de83b01644bb93f625 \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const tokenId = '9b2796d7b4c448de83b01644bb93f625';

const response = await fetch(`https://public-api.getblock.io/api/v1/tokens/${tokenId}`, {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const body = await response.json();

console.log(body.status);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

token_id = "9b2796d7b4c448de83b01644bb93f625"

body = requests.get(
    f"https://public-api.getblock.io/api/v1/tokens/{token_id}",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
).json()

print(body["status"])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
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
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | The access token, the credential inside `endpoint`. Treat it as a secret. |
| `name` | string | Generated name, such as `eth-9b279`. |
| `status` | string | `active`, or `locked` when suspended by plan or billing state. |
| `namespace` | string | Access type the token was issued for, such as `shared`. |
| `protocol` | string | Protocol id. |
| `network` | string | Network id. |
| `api` | string | API type served on the endpoint. |
| `mode` | string | Sync mode. Empty when it does not apply. |
| `region` | string | Region serving the endpoint. |
| `addon` | string | Add-on bound to the token. Empty when none. |
| `dedicated_node_id` | string | Set only for tokens bound to a Dedicated Node. |
| `limitless_node_id` | string | Set only for tokens bound to a Limitless Node. |
| `endpoint` | string | Ready-to-use RPC URL. |
| `created_at`, `updated_at` | string | RFC 3339 UTC times. |

## Use Cases

* Check whether a token is `active` or `locked`
* Look up the endpoint URL and configuration of a token found in a log

## Error Handling

| HTTP | Cause |
| --- | --- |
| 401 | Missing or invalid API key. |
| 404 | No such token among yours. A deleted token, a rotated-away token, and another account's token answer the same way. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
