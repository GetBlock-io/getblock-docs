---
description: >-
  Example code for the bank balances REST method. Complete guide on how to use
  the bank balances REST method in GetBlock Web3 documentation.
---

# bank balances - Cosmos

Returns all token balances held by an address, paginated. Amounts are in the base denomination (uatom).

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
export COSMOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${COSMOS_REST}cosmos/bank/v1beta1/balances/cosmos1xv9tklw7d82sezh9haa573wufgy59vmwe6xxe5"
```
{% endcode %}

## Response

```json
{
    "balances": [
        {
            "denom": "uatom",
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
| balances\[].denom  | string | Base denomination (uatom)                             |
| balances\[].amount | string | Amount in uatom (1 ATOM = 1,000,000 uatom)            |

## Use Cases

* **Balance display**: Show an address's ATOM balance (divide uatom by 1e6)
* **Accounting**: Enumerate all denoms an address holds, including IBC assets

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
