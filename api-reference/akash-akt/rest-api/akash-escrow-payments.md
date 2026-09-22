---
description: >-
  Example code for the akash/escrow/v1beta3/types/payments/list REST method.
  Complete guide on how to use akash/escrow/v1beta3/types/payments/list REST
  method in GetBlock Web3 documentation.
---

# /akash/escrow/v1beta3/types/payments/list - Akash

Returns escrow payments — the streaming payments from escrow accounts to providers for active leases.

## Endpoint

```http
GET /akash/escrow/v1beta3/types/payments/list
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/escrow/v1beta3/types/payments/list"
```
{% endcode %}

## Response

```json
{
    "payments": [
        {
            "account_id": {
                "scope": "deployment",
                "xid": "akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p/12345678"
            },
            "payment_id": "1",
            "owner": "akashvaloper1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
            "state": "open",
            "rate": {
                "denom": "uakt",
                "amount": "1.5"
            },
            "balance": {
                "denom": "uakt",
                "amount": "5000000"
            }
        }
    ],
    "pagination": {
        "total": "1"
    }
}
```

## Response Fields

| Field            | Type   | Description                            |
| ---------------- | ------ | -------------------------------------- |
| payments         | array  | Escrow payments with rate and balance  |
| payments\[].rate | object | Per-block payment rate to the provider |

## Use Cases

* **Billing**: Track provider payouts
* **Analytics**: Measure spend rate

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
