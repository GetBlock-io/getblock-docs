---
description: >-
  Rotate a GetBlock access token: get a replacement with the same
  configuration and a new endpoint URL. Complete guide to POST
  /tokens/{id}/rotate in the Platform API.
---

# Rotate access token - Platform API

`POST /tokens/{id}/rotate` replaces a token with a new one of the same configuration and returns the replacement. Rotation is how a leaked token is retired.

{% code overflow="wrap" %}
```
POST https://public-api.getblock.io/api/v1/tokens/{id}/rotate
```
{% endcode %}

{% hint style="warning" %}
**The replacement has a different `id` and a different `endpoint`.** The old token stops working when the call returns, and its id is no longer found. Store the response and update every place the old endpoint was used.
{% endhint %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Path parameter. The `id` of the token to retire. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://public-api.getblock.io/api/v1/tokens/9b2796d7b4c448de83b01644bb93f625/rotate \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const tokenId = '9b2796d7b4c448de83b01644bb93f625';

const response = await fetch(`https://public-api.getblock.io/api/v1/tokens/${tokenId}/rotate`, {
  method: 'POST',
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const body = await response.json();

console.log(body.endpoint);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

token_id = "9b2796d7b4c448de83b01644bb93f625"

body = requests.post(
    f"https://public-api.getblock.io/api/v1/tokens/{token_id}/rotate",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
).json()

print(body["endpoint"])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

`201 Created`:

{% code overflow="wrap" %}
```json
{
  "id": "4f1c8a2e6b9d47f3a05c2e8b71d4f960",
  "name": "eth-4f1c8",
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
  "endpoint": "https://shared.eu-central-1.getblock.io/4f1c8a2e6b9d47f3a05c2e8b71d4f960/",
  "created_at": "2026-10-15T09:41:07Z",
  "updated_at": "2026-10-15T09:41:07Z"
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

* Retire a token that leaked in a log, a commit, or a client bundle
* Rotate endpoints on a schedule from CI, then redeploy with the new URL in the same job

## Error Handling

| HTTP | Cause |
| --- | --- |
| 401 | Missing or invalid API key. |
| 404 | No such token among yours, or it was already rotated. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
