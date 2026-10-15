---
description: >-
  Create an access token bound to one of your GetBlock Limitless Nodes.
  Complete guide to POST /limitless/{id}/tokens in the Platform API.
---

# Create Limitless Node token - Platform API

`POST /limitless/{id}/tokens` creates an access token bound to one of your Limitless Nodes. The node fixes the protocol, network, region, and sync mode, so the request chooses only the API type and an optional add-on.

{% code overflow="wrap" %}
```
POST https://public-api.getblock.io/api/v1/limitless/{id}/tokens
```
{% endcode %}

{% hint style="info" %}
A Limitless Node routes through shared infrastructure, so any API in the catalog for the node's protocol may be requested, not only those in the node's `apis`. See [`GET /protocols/{id}`](get-protocol.md).
{% endhint %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Path parameter. The node's `id`. |
| `api` | string | Yes | Body parameter. API type, such as `json-rpc` or `ws`. |
| `addon` | string | No | Body parameter. Add-on id, such as `mevprotected`. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://public-api.getblock.io/api/v1/limitless/44a33415-21f6-45a9-b529-91e6503f6c1b/tokens \
  -H "Authorization: Bearer $GETBLOCK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"api":"json-rpc"}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const nodeId = '44a33415-21f6-45a9-b529-91e6503f6c1b';

const response = await fetch(`https://public-api.getblock.io/api/v1/limitless/${nodeId}/tokens`, {
  method: 'POST',
  headers: {
    Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ api: 'json-rpc' })
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

node_id = "44a33415-21f6-45a9-b529-91e6503f6c1b"

body = requests.post(
    f"https://public-api.getblock.io/api/v1/limitless/{node_id}/tokens",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    json={"api": "json-rpc"},
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
  "id": "b83f2e6c0a1d4975be4c7f2a9d36e1c0",
  "name": "eth-b83f2",
  "status": "active",
  "namespace": "limitless",
  "protocol": "eth",
  "network": "mainnet",
  "api": "json-rpc",
  "mode": "full",
  "region": "ap-southeast-1",
  "addon": "",
  "dedicated_node_id": "",
  "limitless_node_id": "44a33415-21f6-45a9-b529-91e6503f6c1b",
  "endpoint": "https://shared.ap-southeast-1.getblock.io/b83f2e6c0a1d4975be4c7f2a9d36e1c0/",
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

* Add a WebSocket endpoint to a Limitless Node
* Issue one token per service on a shared Limitless Node

## Error Handling

| HTTP | Cause |
| --- | --- |
| 400 | `api` is missing, or the API is not served. |
| 401 | Missing or invalid API key. |
| 403 | Your plan does not allow it. |
| 404 | No such node among your Limitless Nodes. |
| 429 | Too many requests. Wait and retry. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
