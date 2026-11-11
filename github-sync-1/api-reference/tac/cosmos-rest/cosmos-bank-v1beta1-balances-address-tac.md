---
description: >-
  Example code for the bank balances JSON-RPC method. Complete guide on how to
  use bank balances JSON-RPC in GetBlock Web3 documentation.
---

# /cosmos/bank/v1beta1/balances/{address} - TAC

Returns all token balances held by an address, paginated. Amounts are in the base denomination.

## Endpoint

```http
GET /cosmos/bank/v1beta1/balances/{address}
```

## Path Parameters

| Parameter | Type   | Description                      |
| --------- | ------ | -------------------------------- |
| address   | string | Bech32 account address (tac1...) |

## Query Parameters

| Parameter        | Type   | Description                            |
| ---------------- | ------ | -------------------------------------- |
| pagination.limit | string | Max results per page                   |
| pagination.key   | string | Next-page key from a previous response |

## Example

{% code overflow="wrap" %}
```bash
export TAC_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TAC_REST}cosmos/bank/v1beta1/balances/tac1ql8dru5dgm6gtg3hcdq9lds3q3fnk3s6k9h8f0"
```
{% endcode %}

## Response

```json
{
    "balances": [
        {
            "denom": "atac",
            "amount": "150000000000000000000"
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
| balances\[].denom  | string | Base denomination (e.g. atac)                         |
| balances\[].amount | string | Amount in the base denomination                       |

## Use Cases

* **Balance display**: Show an address's token balances
* **Accounting**: Enumerate all denoms an address holds

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
