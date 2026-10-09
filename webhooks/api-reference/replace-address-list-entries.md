---
description: >-
  Replace all addresses in one of your address lists. Complete guide on how to
  use PUT /address-lists/{id}/entries in GetBlock Webhooks documentation.
---

# Replace address list entries - Webhooks

`PUT /address-lists/{id}/entries` replaces every address in a list with the ones you send. An empty `addresses` array clears the list. Webhooks that reference the list match the new set at once.

{% code overflow="wrap" %}
```
PUT https://public-api.getblock.io/api/v1/address-lists/{id}/entries
```
{% endcode %}

{% hint style="info" %}
The same limits as for [adding addresses](add-address-list-entries.md) apply: the result is checked against every webhook that references the list, and one failure rejects the whole request.
{% endhint %}

## Parameters

Path parameter:

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | The list's id (`al_…`). |

Body:

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `addresses` | array of strings | Yes | The complete new set, as 0x-prefixed 20-byte hex in any case. Up to 100,000. `[]` clears the list. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X PUT https://public-api.getblock.io/api/v1/address-lists/al_7hK2mPq9Wx3Z/entries \
  -H "Authorization: Bearer $GETBLOCK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"addresses": ["0x742d35cc6634c0532925a3b844bc454e4438f44e", "0x2f1528f344f6410d361654d22a47ef121a60c938"]}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const id = 'al_7hK2mPq9Wx3Z';
const addresses = [
  '0x742d35cc6634c0532925a3b844bc454e4438f44e',
  '0x2f1528f344f6410d361654d22a47ef121a60c938'
]; // for example, every active wallet from your database

const response = await fetch(`https://public-api.getblock.io/api/v1/address-lists/${id}/entries`, {
  method: 'PUT',
  headers: {
    Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ addresses })
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
addresses = [
    "0x742d35cc6634c0532925a3b844bc454e4438f44e",
    "0x2f1528f344f6410d361654d22a47ef121a60c938",
]  # for example, every active wallet from your database

address_list = requests.put(
    f"https://public-api.getblock.io/api/v1/address-lists/{list_id}/entries",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    json={"addresses": addresses},
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
  "entry_count": 2,
  "version": 3,
  "created_at": "2026-09-25T10:00:00Z",
  "updated_at": "2026-09-25T11:00:00Z"
}
```
{% endcode %}

## Response Parameters

The answer is the [address list object](./#the-address-list-object) after the change. `entry_count` is the size of the new set.

## Use Cases

* Sync a list with your own database on a schedule, in one request
* Clear a list with `[]` while keeping its id and the webhooks that reference it

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 400 | `empty_body`, `malformed_body` | No body, invalid JSON, or an unknown field. |
| 400 | `filter_invalid_hex` | An address is not 0x-prefixed 20-byte hex. |
| 404 | `list_not_found` | No such list among yours. |
| 409 | `cap_addresses_per_list_exceeded` | More than 100,000 addresses. |
| 409 | `cap_addresses_exceeded` | A webhook that references the list would exceed its addresses-per-webhook limit. |
| 409 | `cap_account_addresses_exceeded` | Your account's address total would exceed the plan limit. |
| 413 | — | The body is larger than about 5 MiB. |
