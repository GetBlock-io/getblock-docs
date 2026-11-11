---
description: >-
  Add addresses to one of your address lists. Complete guide on how to use POST
  /address-lists/{id}/entries in GetBlock Webhooks documentation.
---

# Add address list entries - Webhooks

`POST /address-lists/{id}/entries` adds addresses to a list. Addresses already in the list are ignored. Webhooks that reference the list match the new addresses at once.

{% code overflow="wrap" %}
```
POST https://public-api.getblock.io/api/v1/address-lists/{id}/entries
```
{% endcode %}

{% hint style="info" %}
Growing a list is checked against the limits of **every webhook that references it** and against your account's total. One failure rejects the whole request, and nothing is added.
{% endhint %}

## Parameters

Path parameter:

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | The list's id (`al_…`). |

Body:

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `addresses` | array of strings | Yes | 0x-prefixed 20-byte hex addresses, in any case. Up to 100,000 per request, within the 5 MiB body limit. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://public-api.getblock.io/api/v1/address-lists/al_7hK2mPq9Wx3Z/entries \
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

const response = await fetch(`https://public-api.getblock.io/api/v1/address-lists/${id}/entries`, {
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
    f"https://public-api.getblock.io/api/v1/address-lists/{list_id}/entries",
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
  "entry_count": 2,
  "version": 2,
  "created_at": "2026-09-25T10:00:00Z",
  "updated_at": "2026-09-25T10:30:00Z"
}
```
{% endcode %}

## Response Parameters

The answer is the [address list object](objects-and-conventions.md#the-address-list-object) after the change. `entry_count` shows the new total.

## Use Cases

* Start watching a new customer deposit address as soon as you issue it
* Import a large set of addresses in batches of up to 100,000

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 400 | `empty_body`, `malformed_body` | No body, invalid JSON, or an unknown field. |
| 400 | `filter_invalid_hex` | An address is not 0x-prefixed 20-byte hex. |
| 404 | `list_not_found` | No such list among yours. |
| 409 | `cap_addresses_per_list_exceeded` | The list would hold more than 100,000 addresses. |
| 409 | `cap_addresses_exceeded` | A webhook that references the list would exceed its addresses-per-webhook limit. |
| 409 | `cap_account_addresses_exceeded` | Your account's address total would exceed the plan limit. |
| 413 | — | The body is larger than about 5 MiB. Send the addresses in several requests. |
