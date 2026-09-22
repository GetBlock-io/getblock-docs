---
description: >-
  Example code for the cosmos/bank/v1beta1/balances/{address} REST method.
  Complete guide on how to use cosmos/bank/v1beta1/balances/{address} REST
  method in GetBlock Web3 documentation.
---

# /cosmos/bank/v1beta1/balances/{address} - Akash

Returns all coin balances held by an address, each as a denom and amount. Native AKT is reported in uakt.

## Endpoint

```http
GET /cosmos/bank/v1beta1/balances/{address}
```

## Path Parameters

| Parameter | Type   | Description                      |
| --------- | ------ | -------------------------------- |
| address   | string | Bech32 account address (akash1…) |

## Query Parameters

| Parameter        | Type   | Description |
| ---------------- | ------ | ----------- |
| pagination.limit | string | Max results |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/bank/v1beta1/balances/akash17xpfvakm2amg962yls6f84z3kell8c5lazw8j8"
```
{% endcode %}

## Response

```json
{
    "balances": [
        {
            "denom": "uakt",
            "amount": "2311523"
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "1"
    }
}
```

## Response Fields

| Field            | Type   | Description                      |
| ---------------- | ------ | -------------------------------- |
| balances         | array  | Coin balances as {denom, amount} |
| pagination.total | string | Total denoms                     |

## Use Cases

* **Balance Reads**: Display AKT and token balances
* **Portfolio**: Enumerate holdings
* **Accounting**: Reconcile balances

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
