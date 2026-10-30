---
description: >-
  Example code for the bank balances REST method. Complete guide on how to use
  bank balances REST method in GetBlock Web3 documentation.
---

# bank balances

Returns all token balances held by an address, paginated. Amounts are in the base denomination (ausdt0).

## Endpoint

```http
GET /cosmos/bank/v1beta1/balances/{address}
```

## Path Parameters

| Parameter | Type   | Description                         |
| --------- | ------ | ----------------------------------- |
| address   | string | Bech32 account address (cosmos1...) |

## Query Parameters

| Parameter        | Type   | Description                            |
| ---------------- | ------ | -------------------------------------- |
| pagination.limit | string | Max results per page                   |
| pagination.key   | string | Next-page key from a previous response |

## Example

{% code overflow="wrap" %}
```bash
export STABLE_REST=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

curl "${STABLE_REST}cosmos/bank/v1beta1/balances/stable1qg5ega6dykkxc307y25pecuufrjkxkagq9k7m4h"
```
{% endcode %}

## Response

```json
{
    "balances": [
        {
            "denom": "ausdt0",
            "amount": "125000000"
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "1"
    }
}
```

## Response Fields

| Field              | Type   | Description                                           |
| ------------------ | ------ | ----------------------------------------------------- |
| balances           | array  | Coins held by the address, each with denom and amount |
| balances\[].denom  | string | Base denomination (ausdt0)                            |
| balances\[].amount | string | Amount in ausdt0 (1 USDT0 = 10^18 ausdt0)             |

## Use Cases

* **Balance display**: Show an address's USDT0 balance (divide ausdt0 by 1e6)
* **Accounting**: Enumerate all denoms an address holds, including IBC assets

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
