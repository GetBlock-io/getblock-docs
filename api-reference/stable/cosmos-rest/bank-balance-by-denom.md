---
description: >-
  Example code for the bank balance by denom REST method. Complete guide on how
  to use bank balance by denom REST method in GetBlock Web3 documentation.
---

# bank balance by denom

Returns the balance of a single denomination for an address.

## Endpoint

```http
GET /cosmos/bank/v1beta1/balances/{address}/by_denom
```

## Path Parameters

| Parameter | Type   | Description                         |
| --------- | ------ | ----------------------------------- |
| address   | string | Bech32 account address (cosmos1...) |

## Query Parameters

| Parameter | Type   | Description                            |
| --------- | ------ | -------------------------------------- |
| denom     | string | The denomination to query, e.g. ausdt0 |

## Example

{% code overflow="wrap" %}
```bash
export STABLE_REST=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

curl "${STABLE_REST}cosmos/bank/v1beta1/balances/stable1qg5ega6dykkxc307y25pecuufrjkxkagq9k7m4h/by_denom"
```
{% endcode %}

## Response

```json
{
    "balance": {
        "denom": "ausdt0",
        "amount": "125000000"
    }
}
```

## Response Fields

| Field          | Type   | Description                     |
| -------------- | ------ | ------------------------------- |
| balance.denom  | string | The queried denomination        |
| balance.amount | string | Amount in the base denomination |

## Use Cases

* **Single-asset read**: Fetch just the USDT0 balance of an address

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
