---
description: >-
  Example code for the fees transaction REST method. Complete guide on how to
  use fees transaction REST method in GetBlock Web3 documentation.
---

# fees transaction - Stacks

Returns approximate fees for a given transaction payload, as low/middle/high estimations, based on the serialized payload and its estimated length.

## Endpoint

```
POST /v2/fees/transaction
```

## Example

{% code overflow="wrap" %}
```bash
export STACKS_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${STACKS_RPC}v2/fees/transaction" \
  -H "Content-Type: application/json" \
  -d '{"transaction_payload": "021af94e...", "estimated_len": 350}'
```
{% endcode %}

## Response

```json
{
    "estimated_cost": {
        "read_count": 10,
        "read_length": 2048,
        "runtime": 100000,
        "write_count": 5,
        "write_length": 512
    },
    "estimated_cost_scalar": 12,
    "estimations": [
        {
            "fee_rate": 1.2,
            "fee": 180
        },
        {
            "fee_rate": 1.5,
            "fee": 240
        },
        {
            "fee_rate": 2.0,
            "fee": 360
        }
    ],
    "cost_scalar_change_by_byte": 0.00476
}
```

## Response Fields

| Field                   | Type   | Description                                                                        |
| ----------------------- | ------ | ---------------------------------------------------------------------------------- |
| estimated\_cost         | object | The transaction's estimated execution cost dimensions                              |
| estimated\_cost\_scalar | number | Scalar summarizing the execution cost                                              |
| estimations             | array  | Fee estimates (low, middle, high), each with a fee\_rate and total fee in microSTX |

## Use Cases

* **Fee tiers**: Offer low/medium/high fee options to a user
* **Contract-call pricing**: Estimate fees for contract-call and deploy transactions

## Error Handling

| Error                     | Message       | Description                                                                  |
| ------------------------- | ------------- | ---------------------------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid or the transaction was rejected |
| 404 / Not found           | Not found     | The account, contract, map entry, variable, or constant does not exist       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect                            |
