---
description: >-
  Create a GetBlock API key, confirm it works, and issue your first access token
  with the Platform API.
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

{% tab title="200 Response" %}
```json
{
  "user_id": "3f6c2a1e-8b4d-4c7a-9e2f-5d1b0a9c8e7f"
}
```
{% endtab %}

{% tab title="401 Response" %}
A `401` means the key is missing, mistyped, or revoked

{% code overflow="wrap" %}
```
authorization failed
```
{% endcode %}
{% endtab %}
{% endtabs %}
{% endstep %}
{% endstepper %}

### Next steps

* [Rotate a token](api-reference/rotate-token.md) when it leaks
* [Check your plan limits](api-reference/get-subscription.md) before creating many tokens
* [Objects and Conventions](api-reference/objects-and-conventions.md): pagination, ownership, and errors
