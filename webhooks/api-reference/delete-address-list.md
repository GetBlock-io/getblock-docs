---
description: >-
  Delete one of your address lists. Complete guide on how to use DELETE
  /address-lists/{id} in GetBlock Webhooks documentation.
---

# Delete address list - Webhooks

`DELETE /address-lists/{id}` deletes a list and its addresses.

{% code overflow="wrap" %}
```
DELETE https://public-api.getblock.io/api/v1/address-lists/{id}
```
{% endcode %}

{% hint style="warning" %}
A list that a webhook still references cannot be deleted (`409 list_in_use`). Remove it from the webhook's `list_refs` with [Update webhook](update-webhook.md), or [delete the webhook](delete-webhook.md), first.
{% endhint %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Path parameter. The list's id (`al_…`). |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X DELETE https://public-api.getblock.io/api/v1/address-lists/al_7hK2mPq9Wx3Z \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const id = 'al_7hK2mPq9Wx3Z';

const response = await fetch(`https://public-api.getblock.io/api/v1/address-lists/${id}`, {
  method: 'DELETE',
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});

if (response.status === 409) {
  console.log('A webhook still references this list');
}
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

list_id = "al_7hK2mPq9Wx3Z"

response = requests.delete(
    f"https://public-api.getblock.io/api/v1/address-lists/{list_id}",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
)

if response.status_code == 409:
    print("A webhook still references this list")
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

`204 No Content`, without a body.

## Response Parameters

None.

## Use Cases

* Free one of your 50 list slots
* Clean up a list after its webhooks moved to another list

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 404 | `list_not_found` | No such list among yours. |
| 409 | `list_in_use` | A webhook still references the list. |
