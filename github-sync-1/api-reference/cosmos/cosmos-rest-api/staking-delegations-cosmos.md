---
description: >-
  Example code for the staking delegations REST method. Complete guide on how to
  use the staking delegations REST method in GetBlock Web3 documentation.
---

# staking delegations - Cosmos

Returns all delegations made by a delegator, paginated.

## Endpoint

```http
GET /cosmos/staking/v1beta1/delegations/{delegator_addr}
```

## Path Parameters

| Parameter       | Type   | Description                           |
| --------------- | ------ | ------------------------------------- |
| delegator\_addr | string | Bech32 delegator address (cosmos1...) |

## Query Parameters

| Parameter        | Type   | Description          |
| ---------------- | ------ | -------------------- |
| pagination.limit | string | Max results per page |

## Example

{% code overflow="wrap" %}
```bash
export COSMOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${COSMOS_REST}cosmos/staking/v1beta1/delegations/cosmos1xv9tklw7d82sezh9haa573wufgy59vmwe6xxe5"
```
{% endcode %}

## Response

```json
{
    "delegation_responses": [
        {
            "delegation": {
                "delegator_address": "cosmos1xv9tklw7d82sezh9haa573wufgy59vmwe6xxe5",
                "validator_address": "cosmosvaloper1xv9tklw7d82sezh9haa573wufgy59vmwnp5s7g",
                "shares": "1000000.000000000000000000"
            },
            "balance": {
                "denom": "uatom",
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

| Field                            | Type   | Description                                               |
| -------------------------------- | ------ | --------------------------------------------------------- |
| delegation\_responses            | array  | The delegator's delegations, each with shares and balance |
| delegation\_responses\[].balance | object | The delegated amount in uatom                             |

## Use Cases

* **Portfolio**: Show a user's staked positions across validators

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
