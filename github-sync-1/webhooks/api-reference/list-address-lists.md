---
description: >-
  List your named address lists, newest first. Complete guide on how to use GET
  /address-lists in GetBlock Webhooks documentation.
---

# List address lists - Webhooks

`GET /address-lists` returns your named address lists, newest first, one page at a time. A webhook's own list, created from its `addresses` field, is not included.

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/address-lists
```
{% endcode %}

## Parameters

Query parameters:

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `limit` | integer | No | Page size. Default 50, at most 200. |
| `cursor` | string | No | The `next_cursor` of the previous page. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl https://public-api.getblock.io/api/v1/address-lists \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const response = await fetch('https://public-api.getblock.io/api/v1/address-lists', {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const { items } = await response.json();

for (const list of items) {
  console.log(list.id, list.name, list.entry_count);
}
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

page = requests.get(
    "https://public-api.getblock.io/api/v1/address-lists",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
).json()

for address_list in page["items"]:
    print(address_list["id"], address_list["name"], address_list["entry_count"])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "items": [
    {
      "id": "al_7hK2mPq9Wx3Z",
      "name": "Treasury wallets",
      "entry_count": 3,
      "version": 2,
      "created_at": "2026-09-25T10:00:00Z",
      "updated_at": "2026-09-25T10:30:00Z"
    }
  ]
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `items` | array | One page of [address list objects](objects-and-conventions.md#the-address-list-object), newest first. Addresses are not included. |
| `next_cursor` | string | Pass it as `cursor` for the next page. Absent on the last page. |

## Use Cases

* Find a list's id by name before referencing it in a webhook
* Check how close each list is to the 100,000-address limit from `entry_count`

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 400 | `invalid_limit`, `invalid_cursor` | `limit` is not an integer, or `cursor` was not issued by this endpoint. |
