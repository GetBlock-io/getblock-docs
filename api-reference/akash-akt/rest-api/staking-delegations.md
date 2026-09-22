# staking delegations

Returns all delegations made by a delegator.

## Endpoint

```
GET /cosmos/staking/v1beta1/delegations/{delegator_addr}
```

## Path Parameters

| Parameter       | Type   | Description                 |
| --------------- | ------ | --------------------------- |
| delegator\_addr | string | Delegator address (akash1…) |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/staking/v1beta1/delegations/akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p"
```
{% endcode %}

## Response

```json
{
    "delegation_responses": [
        {
            "delegation": {
                "delegator_address": "akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
                "validator_address": "akashvaloper1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
                "shares": "1000000.0"
            },
            "balance": {
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

| Field                 | Type  | Description                                   |
| --------------------- | ----- | --------------------------------------------- |
| delegation\_responses | array | Delegations with validator and staked balance |

## Use Cases

* **Portfolio**: Show staking positions

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
