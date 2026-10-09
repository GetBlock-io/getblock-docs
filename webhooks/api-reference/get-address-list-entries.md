---
description: >-
  Read the addresses in one of your address lists. Complete guide on how to use
  GET /address-lists/{id}/entries in GetBlock Webhooks documentation.
---

# Get address list entries - Webhooks

`GET /address-lists/{id}/entries` returns the addresses in a list, one page at a time.

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/address-lists/{id}/entries
```
{% endcode %}

## Parameters

Path parameter:

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | The list's id (`al_…`). |

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
curl "https://public-api.getblock.io/api/v1/address-lists/al_7hK2mPq9Wx3Z/entries?limit=200" \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const id = 'al_7hK2mPq9Wx3Z';
const addresses = [];
let cursor;

do {
  const url = new URL(`https://public-api.getblock.io/api/v1/address-lists/${id}/entries`);
  url.searchParams.set('limit', '200');
  if (cursor) url.searchParams.set('cursor', cursor);

  const response = await fetch(url, {
    headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
  });
  const page = await response.json();

  addresses.push(...page.addresses);
  cursor = page.next_cursor;
} while (cursor);

console.log(addresses.length);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

list_id = "al_7hK2mPq9Wx3Z"
headers = {"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"}
params = {"limit": 200}
addresses = []

while True:
    page = requests.get(
        f"https://public-api.getblock.io/api/v1/address-lists/{list_id}/entries",
        headers=headers,
        params=params,
    ).json()
    addresses.extend(page["addresses"])
    if "next_cursor" not in page:
        break
    params["cursor"] = page["next_cursor"]

print(len(addresses))
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "addresses": [
    "0x2f1528f344f6410d361654d22a47ef121a60c938",
    "0x742d35cc6634c0532925a3b844bc454e4438f44e"
  ],
  "next_cursor": "…"
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `addresses` | array of strings | Lower-case 0x-prefixed addresses, in address order. |
| `next_cursor` | string | Pass it as `cursor` for the next page. Absent on the last page. |

## Use Cases

* Export a list to compare it with your own database
* Show the wallets in a list in your own interface

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 400 | `invalid_limit`, `invalid_cursor` | `limit` is not an integer, or `cursor` was not issued by this endpoint. |
| 404 | `list_not_found` | No such list among yours. |
