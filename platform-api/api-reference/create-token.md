---
description: >-
  Create a GetBlock Shared Node access token and get its endpoint URL.
  Complete guide to POST /tokens in the Platform API.
---

# Create access token - Platform API

`POST /tokens` creates a Shared Node access token and returns it with its ready-to-use endpoint URL. The token's `id` and `name` are generated.

{% code overflow="wrap" %}
```
POST https://public-api.getblock.io/api/v1/tokens
```
{% endcode %}

{% hint style="info" %}
The combination of `protocol`, `network`, `mode`, `api`, and `region` must be one GetBlock serves. Read valid combinations from [`GET /protocols/{id}`](get-protocol.md). For a token on a Dedicated or Limitless Node, use [`POST /dedicated/{id}/tokens`](create-dedicated-node-token.md) or [`POST /limitless/{id}/tokens`](create-limitless-node-token.md).
{% endhint %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `protocol` | string | Yes | Protocol id from [`GET /protocols`](list-protocols.md), such as `eth`. |
| `network` | string | Yes | Network id, such as `mainnet`. |
| `api` | string | Yes | API type, such as `json-rpc`, `ws`, or `graphql`. |
| `mode` | string | No | Sync mode, `full` or `archive`. Default `full`. |
| `region` | string | No | Region, such as `eu-central-1`. Default `eu-central-1`. |
| `addon` | string | No | Add-on id from [`GET /addons`](list-addons.md), such as `mevprotected`. |

Body parameters, sent as JSON.

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://public-api.getblock.io/api/v1/tokens \
  -H "Authorization: Bearer $GETBLOCK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"protocol":"eth","network":"mainnet","api":"json-rpc","mode":"archive","region":"eu-central-1"}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const response = await fetch('https://public-api.getblock.io/api/v1/tokens', {
  method: 'POST',
  headers: {
    Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ protocol: 'eth', network: 'mainnet', api: 'json-rpc', mode: 'archive', region: 'eu-central-1' })
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

body = requests.post(
    "https://public-api.getblock.io/api/v1/tokens",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    json={"protocol": "eth", "network": "mainnet", "api": "json-rpc", "mode": "archive", "region": "eu-central-1"},
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

* Provision an endpoint per environment from a deployment script
* Give each customer or service its own token, so one can be rotated without touching the others
* Create an MEV-protected endpoint by passing `"addon": "mevprotected"`

## Error Handling

| HTTP | Cause |
| --- | --- |
| 400 | A required field is missing, or the combination is not served. The `error` field says which. |
| 401 | Missing or invalid API key. |
| 403 | Your plan does not allow it: the token limit is reached, or the network or region needs a paid plan. |
| 429 | Too many requests. Wait and retry. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
