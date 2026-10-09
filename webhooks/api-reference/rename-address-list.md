---
description: >-
  Rename one of your address lists. Complete guide on how to use PATCH
  /address-lists/{id} in GetBlock Webhooks documentation.
---

# Rename address list - Webhooks

`PATCH /address-lists/{id}` changes a list's name. Its addresses are managed through the `entries` endpoints, and webhooks that reference the list are not affected: they reference it by id.

{% code overflow="wrap" %}
```
PATCH https://public-api.getblock.io/api/v1/address-lists/{id}
```
{% endcode %}

## Parameters

Path parameter:

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | The list's id (`al_…`). |

Body:

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | string | Yes | The new name. Unique per account, at most 128 bytes. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X PATCH https://public-api.getblock.io/api/v1/address-lists/al_7hK2mPq9Wx3Z \
  -H "Authorization: Bearer $GETBLOCK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"name": "Cold wallets"}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const id = 'al_7hK2mPq9Wx3Z';

const response = await fetch(`https://public-api.getblock.io/api/v1/address-lists/${id}`, {
  method: 'PATCH',
  headers: {
    Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ name: 'Cold wallets' })
});

console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

list_id = "al_7hK2mPq9Wx3Z"

address_list = requests.patch(
    f"https://public-api.getblock.io/api/v1/address-lists/{list_id}",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    json={"name": "Cold wallets"},
).json()

print(address_list)
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "id": "al_7hK2mPq9Wx3Z",
  "name": "Cold wallets",
  "entry_count": 3,
  "version": 3,
  "created_at": "2026-09-25T10:00:00Z",
  "updated_at": "2026-09-25T11:00:00Z"
}
```
{% endcode %}

## Response Parameters

The answer is the renamed [address list object](./#the-address-list-object).

## Use Cases

* Give a list a clearer name after its purpose changes
* Free a name so another list can use it

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 400 | `empty_body`, `malformed_body` | No body, invalid JSON, or an unknown field. |
| 400 | `invalid_list_name` | `name` is empty after trimming, over 128 bytes, or has control characters. |
| 400 | `list_name_taken` | You already have a list with this name. |
| 404 | `list_not_found` | No such list among yours. |
