---
description: >-
  Create a GetBlock API key, confirm it works, and issue your first access
  token with the Platform API.
---

# Getting Started

This page takes you from a new API key to a working RPC endpoint created through the Platform API.

### Prerequisites

* A [GetBlock account](https://account.getblock.io/sign-in)
* `curl`, or Node.js 18+ / Python 3 for the code examples

{% stepper %}
{% step %}
### Create an API key

1. Sign in to the [GetBlock dashboard](https://account.getblock.io/).
2. Go to [**Settings → API Keys**](https://account.getblock.io/settings/api-keys).
3. Create a key and copy it. Keys start with `gb_`.

API keys can only be created in the dashboard, not through the API.
{% endstep %}

{% step %}
### Store the key in an environment variable

The examples in these docs read the key from `GETBLOCK_API_KEY`:

```bash
export GETBLOCK_API_KEY="gb_..."
```

{% hint style="warning" %}
The key acts on your whole account, token deletion included. Keep it out of source code and version control.
{% endhint %}
{% endstep %}

{% step %}
### Confirm the key works

Call [`GET /me`](api-reference/get-me.md). It returns your user id when the key is valid:

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl https://public-api.getblock.io/api/v1/me \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="Response" %}
```json
{
  "user_id": "3f6c2a1e-8b4d-4c7a-9e2f-5d1b0a9c8e7f"
}
```
{% endtab %}
{% endtabs %}

A `401` means the key is missing, mistyped, or revoked.
{% endstep %}

{% step %}
### Find a valid configuration

A token needs a `protocol`, `network`, and `api` that GetBlock serves together. Read them from the catalog with [`GET /protocols/{id}`](api-reference/get-protocol.md):

{% code overflow="wrap" %}
```bash
curl https://public-api.getblock.io/api/v1/protocols/eth \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}

Read the response downwards: pick a network, then a sync mode it offers, then an API in that mode, then a region listed for that API.
{% endstep %}

{% step %}
### Create an access token

Send the configuration to [`POST /tokens`](api-reference/create-token.md):

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://public-api.getblock.io/api/v1/tokens \
  -H "Authorization: Bearer $GETBLOCK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"protocol":"eth","network":"mainnet","api":"json-rpc","mode":"archive"}'
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
  body: JSON.stringify({ protocol: 'eth', network: 'mainnet', api: 'json-rpc', mode: 'archive' })
});
const token = await response.json();

console.log(token.endpoint);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

token = requests.post(
    "https://public-api.getblock.io/api/v1/tokens",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    json={"protocol": "eth", "network": "mainnet", "api": "json-rpc", "mode": "archive"},
).json()

print(token["endpoint"])
```
{% endcode %}
{% endtab %}
{% endtabs %}

The response is the new token, with its ready-to-use `endpoint`:

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
{% endstep %}

{% step %}
### Call the new endpoint

The endpoint is a normal GetBlock RPC URL:

{% code overflow="wrap" %}
```bash
curl -X POST https://shared.eu-central-1.getblock.io/9b2796d7b4c448de83b01644bb93f625/ \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":"getblock.io"}'
```
{% endcode %}

{% hint style="info" %}
The token `id` is the credential inside the endpoint URL. Treat it as a secret, the same as the API key.
{% endhint %}
{% endstep %}
{% endstepper %}

### Next steps

* [Rotate a token](api-reference/rotate-token.md) when it leaks
* [Check your plan limits](api-reference/get-subscription.md) before creating many tokens
* [Objects and Conventions](api-reference/objects-and-conventions.md): pagination, ownership, and errors
