---
description: >-
  Remove addresses from one of your address lists. Complete guide on how to use
  POST /address-lists/{id}/entries/remove in GetBlock Webhooks documentation.
---

# Remove address list entries - Webhooks

`POST /address-lists/{id}/entries/remove` removes the addresses you send from a list. Addresses that are not in the list are ignored. Webhooks that reference the list stop matching the removed addresses at once.

{% code overflow="wrap" %}
```
POST https://public-api.getblock.io/api/v1/address-lists/{id}/entries/remove
```
{% endcode %}

{% hint style="info" %}
It is a `POST`, not a `DELETE`, because a large set of addresses does not fit in a query string.
{% endhint %}

## Parameters

Path parameter:

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | The list's id (`al_…`). |

Body:

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `addresses` | array of strings | Yes | 0x-prefixed 20-byte hex addresses, in any case. Up to 100,000 per request. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://public-api.getblock.io/api/v1/address-lists/al_7hK2mPq9Wx3Z/entries/remove \
  -H "Authorization: Bearer $GETBLOCK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"addresses": ["0x2f1528f344f6410d361654d22a47ef121a60c938"]}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const id = 'al_7hK2mPq9Wx3Z';

const response = await fetch(`https://public-api.getblock.io/api/v1/address-lists/${id}/entries/remove`, {
  method: 'POST',
  headers: {
    Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ addresses: ['0x2f1528f344f6410d361654d22a47ef121a60c938'] })
});
const list = await response.json();

console.log(list.entry_count);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

list_id = "al_7hK2mPq9Wx3Z"

address_list = requests.post(
    f"https://public-api.getblock.io/api/v1/address-lists/{list_id}/entries/remove",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    json={"addresses": ["0x2f1528f344f6410d361654d22a47ef121a60c938"]},
).json()

print(address_list["entry_count"])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "id": "al_7hK2mPq9Wx3Z",
  "name": "Treasury wallets",
  "entry_count": 1,
  "version": 4,
  "created_at": "2026-09-25T10:00:00Z",
  "updated_at": "2026-09-25T11:30:00Z"
}
```
{% endcode %}

## Response Parameters

The answer is the [address list object](objects-and-conventions.md#the-address-list-object) after the change. `entry_count` shows the new total.

## Use Cases

* Stop watching a wallet you retired
* Remove addresses of a customer who closed their account

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 400 | `empty_body`, `malformed_body` | No body, invalid JSON, or an unknown field. |
| 400 | `filter_invalid_hex` | An address is not 0x-prefixed 20-byte hex. |
| 404 | `list_not_found` | No such list among yours. |
| 413 | — | The body is larger than about 5 MiB. |
