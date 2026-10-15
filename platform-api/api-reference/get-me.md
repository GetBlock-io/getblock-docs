---
description: >-
  Return the user id behind a GetBlock API key. Use GET /me to check that a
  Platform API key is valid.
---

# Get caller identity - Platform API

`GET /me` returns the id of the user the API key belongs to. Call it first to check that a new key works.

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/me
```
{% endcode %}

## Parameters

None.

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl https://public-api.getblock.io/api/v1/me \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const response = await fetch('https://public-api.getblock.io/api/v1/me', {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const body = await response.json();

console.log(body.user_id);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

body = requests.get(
    "https://public-api.getblock.io/api/v1/me",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
).json()

print(body["user_id"])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "user_id": "3f6c2a1e-8b4d-4c7a-9e2f-5d1b0a9c8e7f"
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `user_id` | string (UUID) | Your GetBlock user id. |

## Use Cases

* Check that an API key works before a deployment job uses it
* Health-check the key in a monitoring job: a `401` means it was revoked

## Error Handling

| HTTP | Cause |
| --- | --- |
| 401 | Missing, mistyped, or revoked API key. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
