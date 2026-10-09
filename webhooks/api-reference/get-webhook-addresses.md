---
description: >-
  Read the addresses a webhook was given in its addresses field. Complete guide
  on how to use GET /webhooks/{id}/addresses in GetBlock Webhooks
  documentation.
---

# Get webhook addresses - Webhooks

`GET /webhooks/{id}/addresses` returns the addresses the webhook was created or updated with in `addresses`, one page at a time, together with the id of the list that holds them.

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/webhooks/{id}/addresses
```
{% endcode %}

{% hint style="info" %}
That list belongs to the webhook. It does not appear under [`GET /address-lists`](list-address-lists.md) and changes only through [`PATCH /webhooks/{id}`](update-webhook.md) with `addresses`. Addresses from named lists in `list_refs` are not included: read them with [Get address list entries](get-address-list-entries.md).
{% endhint %}

## Parameters

Path parameter:

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | The webhook's id (`wh_…`). |

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
curl "https://public-api.getblock.io/api/v1/webhooks/wh_4Qk7Xz9pLm2R/addresses?limit=200" \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const id = 'wh_4Qk7Xz9pLm2R';
const addresses = [];
let cursor;

do {
  const url = new URL(`https://public-api.getblock.io/api/v1/webhooks/${id}/addresses`);
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

webhook_id = "wh_4Qk7Xz9pLm2R"
headers = {"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"}
params = {"limit": 200}
addresses = []

while True:
    page = requests.get(
        f"https://public-api.getblock.io/api/v1/webhooks/{webhook_id}/addresses",
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
  "list_id": "al_2bQ4rTz9KmNp",
  "addresses": ["0x742d35cc6634c0532925a3b844bc454e4438f44e"]
}
```
{% endcode %}

A webhook without `addresses` answers `{"list_id": null, "addresses": []}`.

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `list_id` | string or null | The id of the webhook's own list. The webhook's filter references it in `list_refs`. `null` when the webhook has no `addresses`. |
| `addresses` | array of strings | Lower-case 0x-prefixed addresses, in address order. Never `null`. |
| `next_cursor` | string | Pass it as `cursor` for the next page. Absent on the last page. |

## Use Cases

* Show which wallets a webhook watches in your own interface
* Compare a webhook's addresses with your database before replacing them with [`PATCH`](update-webhook.md)

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 400 | `invalid_limit`, `invalid_cursor` | `limit` is not an integer, or `cursor` was not issued by this endpoint. |
| 404 | `not_found` | No such webhook among yours. |
