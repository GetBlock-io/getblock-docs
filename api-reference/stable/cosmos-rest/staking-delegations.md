---
description: >-
  Example code for the staking delegations REST method. Complete guide on how to
  use staking delegations REST method in GetBlock Web3 documentation.
---

# staking delegations

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
export STABLE_REST=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

curl "${STABLE_REST}cosmos/staking/v1beta1/delegations/stable1qg5ega6dykkxc307y25pecuufrjkxkagq9k7m4h"
```
{% endcode %}

## Response

```json
{
    "delegation_responses": [
        {
            "delegation": {
                "delegator_address": "stable1qg5ega6dykkxc307y25pecuufrjkxkagq9k7m4h",
                "validator_address": "stablevaloper1qg5ega6dykkxc307y25pecuufrjkxkagqe8n2y5",
                "shares": "1000000.000000000000000000"
            },
            "balance": {
                "denom": "ausdt0",
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
| delegation\_responses\[].balance | object | The delegated amount in ausdt0                            |

## Use Cases

* **Portfolio**: Show a user's staked positions across validators

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
