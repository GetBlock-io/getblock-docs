---
description: >-
  Get one of your address lists by id, without its addresses. Complete guide on
  how to use GET /address-lists/{id} in GetBlock Webhooks documentation.
---

# Get address list - Webhooks

`GET /address-lists/{id}` returns one of your address lists, without its addresses. Read the addresses with [Get address list entries](get-address-list-entries.md).

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/address-lists/{id}
```
{% endcode %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Path parameter. The list's id (`al_…`). |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl https://public-api.getblock.io/api/v1/address-lists/al_7hK2mPq9Wx3Z \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const id = 'al_7hK2mPq9Wx3Z';

const response = await fetch(`https://public-api.getblock.io/api/v1/address-lists/${id}`, {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
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

address_list = requests.get(
    f"https://public-api.getblock.io/api/v1/address-lists/{list_id}",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
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
  "name": "Treasury wallets",
  "entry_count": 3,
  "version": 2,
  "created_at": "2026-09-25T10:00:00Z",
  "updated_at": "2026-09-25T10:30:00Z"
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | `al_` followed by 12 characters. |
| `name` | string | Unique per account, at most 128 bytes. |
| `entry_count` | integer | How many addresses the list holds. |
| `version` | integer | The list's version number. Compare it to detect changes. |
| `created_at`, `updated_at` | string | RFC 3339 times. |

## Use Cases

* Check `entry_count` after a large import
* Detect a change made elsewhere by comparing `version`

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 404 | `list_not_found` | No such list among yours. Another account's list and a malformed id answer the same way. |
