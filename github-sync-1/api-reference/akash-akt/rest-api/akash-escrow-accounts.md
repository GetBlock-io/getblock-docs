---
description: >-
  Example code for the akash/escrow/v1beta3/types/accounts/list REST method.
  Complete guide on how to use akash/escrow/v1beta3/types/accounts/list REST
  method in GetBlock Web3 documentation.
---

# /akash/escrow/v1beta3/types/accounts/list - Akash

Returns escrow accounts, which hold funds that pay for deployments and leases over time.

{% hint style="danger" %}
**This endpoint is not available on GetBlock's Akash REST endpoint.** Every request returns `501 Not Implemented`:

```json
{
    "jsonrpc": "",
    "error": {
        "code": -32701,
        "message": "not implemented"
    }
}
```

The gateway returns that error for any path it does not route, and no module version resolves it: `v1`, `v1beta1` through `v1beta5` were all tried. The other Akash modules — deployment, market, provider, cert, and audit — do respond, so this is specific to the module below rather than to Akash paths in general.
{% endhint %}

## Endpoint

```http
GET /akash/escrow/v1beta3/types/accounts/list
```

## Query Parameters

| Parameter | Type   | Description                |
| --------- | ------ | -------------------------- |
| scope     | string | Escrow scope (deployment)  |
| xid       | string | Escrow account external id |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/escrow/v1beta3/types/accounts/list?scope=deployment&xid="
```
{% endcode %}

## Response

```json
{
    "accounts": [
        {
            "id": {
                "scope": "deployment",
                "xid": "akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p/12345678"
            },
            "owner": "akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
            "state": "open",
            "balance": {
                "denom": "uakt",
                "amount": "5000000"
            },
            "transferred": {
                "denom": "uakt",
                "amount": "1000000"
            }
        }
    ],
    "pagination": {
        "total": "1"
    }
}
```

## Response Fields

| Field                   | Type   | Description                                    |
| ----------------------- | ------ | ---------------------------------------------- |
| accounts                | array  | Escrow accounts with owner, state, and balance |
| accounts\[].balance     | object | Remaining escrow balance                       |
| accounts\[].transferred | object | Amount already paid out                        |

## Use Cases

* **Billing**: Track deployment funding
* **Monitoring**: Alert on low escrow

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
