---
description: >-
  Issue a new signing secret for a webhook, with or without a 24-hour overlap.
  Complete guide on how to use POST /webhooks/{id}/secret/rotate in GetBlock
  Webhooks documentation.
---

# Rotate webhook secret - Webhooks

`POST /webhooks/{id}/secret/rotate` issues a new signing secret and returns it. By default the previous secret keeps working for 24 hours: during that overlap every delivery carries signatures made with both secrets, so your endpoint can switch without missing a delivery.

{% code overflow="wrap" %}
```
POST https://public-api.getblock.io/api/v1/webhooks/{id}/secret/rotate
```
{% endcode %}

{% hint style="warning" %}
**The new secret is shown only in this answer.** Store it before you do anything else.
{% endhint %}

## Parameters

Path parameter:

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | The webhook's id (`wh_…`). |

The body is optional:

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `expire_previous` | boolean | No | `false` (the default): the previous secret works for 24 more hours. `true`: the previous secret stops working at once. Use it when the secret has leaked. |

A second rotation inside the 24-hour overlap retires the older secret at once.

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
# Routine rotation, 24-hour overlap
curl -X POST https://public-api.getblock.io/api/v1/webhooks/wh_4Qk7Xz9pLm2R/secret/rotate \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"

# Leaked secret: retire the previous one at once
curl -X POST https://public-api.getblock.io/api/v1/webhooks/wh_4Qk7Xz9pLm2R/secret/rotate \
  -H "Authorization: Bearer $GETBLOCK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"expire_previous": true}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const id = 'wh_4Qk7Xz9pLm2R';

const response = await fetch(`https://public-api.getblock.io/api/v1/webhooks/${id}/secret/rotate`, {
  method: 'POST',
  headers: {
    Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ expire_previous: false })
});
const { secret, previous_expires_at } = await response.json();

// Save `secret` to your secret store, then accept both secrets until previous_expires_at
console.log(previous_expires_at);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

webhook_id = "wh_4Qk7Xz9pLm2R"

rotation = requests.post(
    f"https://public-api.getblock.io/api/v1/webhooks/{webhook_id}/secret/rotate",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    json={"expire_previous": False},
).json()

# Save rotation["secret"] to your secret store, then accept both secrets until previous_expires_at
print(rotation["previous_expires_at"])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "secret": "whsec_8Zt3kW1qP5nR7yL2vB9cX4mH6",
  "version": 4,
  "previous_expires_at": "2026-09-26T10:00:00Z"
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `secret` | string | The new signing secret (`whsec_…`). **Shown only in this answer.** |
| `version` | integer | The webhook's new `version`. |
| `previous_expires_at` | string or null | When the previous secret stops working. `null` when there is no overlap, as with `"expire_previous": true`. |

The full procedure, including how to accept two secrets at once, is in [Rotating the secret](../verifying-signatures.md#rotating-the-secret).

## Use Cases

* Replace a secret you lost
* Retire a leaked secret at once with `"expire_previous": true`
* Rotate secrets on a schedule as part of your security policy

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 400 | `malformed_body` | The body is not a valid rotation request, for example an unknown field. |
| 404 | `not_found` | No such webhook among yours. |
