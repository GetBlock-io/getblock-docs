---
description: >-
  Create an access token bound to one of your GetBlock Dedicated Nodes.
  Complete guide to POST /dedicated/{id}/tokens in the Platform API.
---

# Create Dedicated Node token - Platform API

`POST /dedicated/{id}/tokens` creates an access token bound to one of your Dedicated Nodes and returns it with its endpoint URL. The node fixes the protocol, network, and region, so the request chooses only the API type and an optional add-on.

{% code overflow="wrap" %}
```
POST https://public-api.getblock.io/api/v1/dedicated/{id}/tokens
```
{% endcode %}

{% hint style="info" %}
Send `api` and `addon` values exactly as the node lists them in `apis` and `addons` from [`GET /dedicated/{id}`](get-dedicated-node.md).
{% endhint %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Path parameter. The node's `id`. |
| `api` | string | Yes | Body parameter. One of the node's `apis`, such as `jsonrpc`. |
| `addon` | string | No | Body parameter. One of the node's `addons`. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://public-api.getblock.io/api/v1/dedicated/abc123/tokens \
  -H "Authorization: Bearer $GETBLOCK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"api":"jsonrpc"}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const nodeId = 'abc123';

const response = await fetch(`https://public-api.getblock.io/api/v1/dedicated/${nodeId}/tokens`, {
  method: 'POST',
  headers: {
    Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ api: 'jsonrpc' })
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

node_id = "abc123"

body = requests.post(
    f"https://public-api.getblock.io/api/v1/dedicated/{node_id}/tokens",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    json={"api": "jsonrpc"},
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
  "id": "7d0e3b5a91c24f8e9b6a2d4c08f1e357",
  "name": "eth-7d0e3",
  "status": "active",
  "namespace": "dedicated",
  "protocol": "eth",
  "network": "mainnet",
  "api": "jsonrpc",
  "mode": "archive",
  "region": "eu-central-1",
  "addon": "",
  "dedicated_node_id": "abc123",
  "limitless_node_id": "",
  "endpoint": "https://shared.eu-central-1.getblock.io/7d0e3b5a91c24f8e9b6a2d4c08f1e357/",
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

* Issue separate endpoints on one node for each service, so each can be rotated alone
* Add a WebSocket endpoint to a node that so far only has JSON-RPC

## Error Handling

| HTTP | Cause |
| --- | --- |
| 400 | `api` is missing, or the node does not serve it. |
| 401 | Missing or invalid API key. |
| 403 | Your plan does not allow it. |
| 404 | No such node among your Dedicated Nodes. |
| 429 | Too many requests. Wait and retry. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
