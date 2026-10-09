---
description: >-
  Delete a webhook and free its plan slot. Complete guide on how to use DELETE
  /webhooks/{id} in GetBlock Webhooks documentation.
---

# Delete webhook - Webhooks

`DELETE /webhooks/{id}` deletes a webhook. Deliveries stop and the webhook frees its plan slot and its addresses from your account's address count.

{% code overflow="wrap" %}
```
DELETE https://public-api.getblock.io/api/v1/webhooks/{id}
```
{% endcode %}

{% hint style="danger" %}
**This cannot be undone.** The webhook, its stats, and its delivery history are no longer reachable. To stop deliveries for a while, [pause](pause-webhook.md) the webhook instead.
{% endhint %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Path parameter. The webhook's id (`wh_…`). |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X DELETE https://public-api.getblock.io/api/v1/webhooks/wh_4Qk7Xz9pLm2R \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const id = 'wh_4Qk7Xz9pLm2R';

const response = await fetch(`https://public-api.getblock.io/api/v1/webhooks/${id}`, {
  method: 'DELETE',
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});

console.log(response.status); // 204
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

webhook_id = "wh_4Qk7Xz9pLm2R"

response = requests.delete(
    f"https://public-api.getblock.io/api/v1/webhooks/{webhook_id}",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
)

print(response.status_code)  # 204
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

`204 No Content`, without a body.

## Response Parameters

None.

## Use Cases

* Free a slot when you reach your plan's active-webhook limit (a paused webhook still holds its slot)
* Remove a webhook whose `chain`, `network`, or `trigger_type` must change, before creating its replacement
* Clean up webhooks of a customer who left your service

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 404 | `not_found` | No such webhook among yours, or it is already deleted. |
