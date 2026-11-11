---
description: >-
  Get your plan's webhook limits and current address usage. Complete guide on
  how to use GET /webhooks/limits in GetBlock Webhooks documentation.
---

# Get webhook limits - Webhooks

`GET /webhooks/limits` returns the limits your next create or update is checked against, and how many addresses your account already watches. Call it to check whether a webhook fits your plan before you create it.

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/webhooks/limits
```
{% endcode %}

{% hint style="info" %}
The answer is advisory: your plan can change between this call and the next create. The create's own answer is authoritative.
{% endhint %}

## Parameters

None.

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl https://public-api.getblock.io/api/v1/webhooks/limits \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const response = await fetch('https://public-api.getblock.io/api/v1/webhooks/limits', {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const { caps, usage } = await response.json();

const free = caps.addresses_per_account === 0
  ? Infinity
  : caps.addresses_per_account - usage.account_addresses;
console.log(`Addresses you can still add: ${free}`);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

limits = requests.get(
    "https://public-api.getblock.io/api/v1/webhooks/limits",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
).json()

caps, usage = limits["caps"], limits["usage"]
if caps["addresses_per_account"] == 0:
    print("No account-wide address limit")
else:
    print("Addresses you can still add:", caps["addresses_per_account"] - usage["account_addresses"])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "tier": "pro",
  "account_type": "personal",
  "plan_required": false,
  "caps": {
    "active_webhooks": 75,
    "addresses_per_webhook": 250000,
    "inline_addresses": 1000,
    "list_refs": 10,
    "addresses_per_list": 100000,
    "address_lists": 50,
    "addresses_per_account": 1000000,
    "events_per_second": 30,
    "events_burst": 360
  },
  "usage": { "account_addresses": 3 }
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `tier` | string | Your plan tier. |
| `account_type` | string | `personal` or a team account. |
| `plan_required` | boolean | `true` for a team account without a plan. Creating a webhook or an address list then answers `403 plan_required`. |
| `caps.active_webhooks` | integer | Webhooks the account can hold, paused ones included. |
| `caps.addresses_per_webhook` | integer | Addresses one webhook can watch, its own and its lists' together, without de-duplication. |
| `caps.inline_addresses` | integer | Addresses written into one webhook's filter. |
| `caps.list_refs` | integer | Address lists one webhook can reference. |
| `caps.addresses_per_list` | integer | Addresses in one address list. |
| `caps.address_lists` | integer | Address lists the account can hold. |
| `caps.addresses_per_account` | integer | Addresses all your webhooks can watch together, a shared list counted once per webhook. `0` means no limit. |
| `caps.events_per_second` | integer | Events per second delivered to all your webhooks. Events above the rate and burst are dropped. `0` means the platform default applies. |
| `caps.events_burst` | integer | Events that can go out at once before `events_per_second` applies. `0` means the platform default applies. |
| `usage.account_addresses` | integer | Addresses your account watches now, counted the same way as `addresses_per_account`. |

The values for each plan are in [Plan limits](../pricing-and-limits.md#plan-limits).

## Use Cases

* Check that a large address import fits before you send it
* Show remaining capacity in your own dashboard
* Detect a team account that needs a plan before the first create fails

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 503 | `plan_caps_unavailable` | Your plan could not be checked right now. Retry. |
