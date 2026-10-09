---
description: >-
  Create a named address list that several webhooks can reference. Complete
  guide on how to use POST /address-lists in GetBlock Webhooks documentation.
---

# Create address list - Webhooks

`POST /address-lists` creates a named address list, optionally with its first addresses. Any number of webhooks can then watch the list through `list_refs`, and changing the list changes what all of them match without editing the webhooks.

{% code overflow="wrap" %}
```
POST https://public-api.getblock.io/api/v1/address-lists
```
{% endcode %}

{% hint style="info" %}
Use a list when you watch more than the 1,000 addresses a webhook's filter can hold, or when several webhooks share the same addresses. A list holds up to 100,000 addresses, and an account can have 50 lists. See [Address lists](../triggers-and-filters.md#address-lists).
{% endhint %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | string | Yes | Unique per account, at most 128 bytes. Surrounding spaces are trimmed. |
| `addresses` | array of strings | No | First addresses, as 0x-prefixed 20-byte hex in any case. Up to 100,000 per request. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://public-api.getblock.io/api/v1/address-lists \
  -H "Authorization: Bearer $GETBLOCK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"name": "Treasury wallets", "addresses": ["0x742d35cc6634c0532925a3b844bc454e4438f44e"]}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const response = await fetch('https://public-api.getblock.io/api/v1/address-lists', {
  method: 'POST',
  headers: {
    Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    name: 'Treasury wallets',
    addresses: ['0x742d35cc6634c0532925a3b844bc454e4438f44e']
  })
});
const list = await response.json();

console.log(list.id); // use it in a webhook's list_refs
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

address_list = requests.post(
    "https://public-api.getblock.io/api/v1/address-lists",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    json={
        "name": "Treasury wallets",
        "addresses": ["0x742d35cc6634c0532925a3b844bc454e4438f44e"],
    },
).json()

print(address_list["id"])  # use it in a webhook's list_refs
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

`201 Created`:

{% code overflow="wrap" %}
```json
{
  "id": "al_7hK2mPq9Wx3Z",
  "name": "Treasury wallets",
  "entry_count": 1,
  "version": 1,
  "created_at": "2026-09-25T10:00:00Z",
  "updated_at": "2026-09-25T10:00:00Z"
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | The list's id (`al_…`). Send it in a webhook's `list_refs`. |
| `name` | string | The list's name, trimmed. |
| `entry_count` | integer | How many addresses the list holds. |
| `version` | integer | `1`. |
| `created_at`, `updated_at` | string | RFC 3339 times. |

To watch the list, send its id when you [create](create-webhook.md) or [update](update-webhook.md) a webhook: `"list_refs": ["al_7hK2mPq9Wx3Z"]`.

## Use Cases

* Keep all deposit addresses of an exchange in one place for several webhooks
* Watch more than 1,000 addresses with one webhook
* Separate hot and cold wallets into lists that different webhooks reference

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 400 | `empty_body`, `malformed_body` | No body, invalid JSON, or an unknown field. |
| 400 | `invalid_list_name` | `name` is empty after trimming, over 128 bytes, or has control characters. |
| 400 | `list_name_taken` | You already have a list with this name. |
| 400 | `filter_invalid_hex` | An address is not 0x-prefixed 20-byte hex. |
| 403 | `plan_required` | A team account without a plan. |
| 409 | `cap_address_lists_exceeded` | You already have 50 lists. |
| 409 | `cap_addresses_per_list_exceeded` | More than 100,000 addresses. |
| 413 | — | The body is larger than about 5 MiB. |
