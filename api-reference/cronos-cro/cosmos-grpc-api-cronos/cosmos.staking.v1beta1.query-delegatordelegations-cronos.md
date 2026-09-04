---
description: >-
  Example code for the cosmos.staking.v1beta1.Query/DelegatorDelegations gRPC
  method. Complete guide on how to use
  cosmos.staking.v1beta1.Query/DelegatorDelegations gRPC method in GetBlock Web3
  documentation.
---

# cosmos.staking.v1beta1.Query/DelegatorDelegations - Cronos

Returns all delegations made by a delegator address, each with the validator and the staked amount.

## Service Method

```
cosmos.staking.v1beta1.Query/DelegatorDelegations
```

## Message Fields

| Field           | Type   | Description                      |
| --------------- | ------ | -------------------------------- |
| delegator\_addr | string | Delegator bech32 address (crc1…) |
| pagination      | object | Optional pagination              |

## Example

{% code overflow="wrap" %}
```bash
grpcurl \
  -H "x-api-key: <ACCESS-TOKEN>" \
  -d '{
    "delegator_addr": "crc1p9zq0lr6h6n5s2wk9m3v7t4x8c6b2d0f1g3h5j"
}' \
  shared.eu-central-1.getblock.io:443 \
  cosmos.staking.v1beta1.Query/DelegatorDelegations
```
{% endcode %}

## Response

```json
{
    "delegation_responses": [
        {
            "delegation": {
                "delegator_address": "crc1p9zq0lr6h6n5s2wk9m3v7t4x8c6b2d0f1g3h5j",
                "validator_address": "crcvaloper1p9zq0lr6h6n5s2wk9m3v7t4x8c6b2d0f1g3h5j",
                "shares": "1000000.000000000000000000"
            },
            "balance": {
                "denom": "basecro",
                "amount": "1000000000000000000000"
            }
        }
    ],
    "pagination": {
        "total": "1"
    }
}
```

## Response Fields

| Field                            | Type   | Description                                   |
| -------------------------------- | ------ | --------------------------------------------- |
| delegation\_responses            | array  | Delegations with validator and staked balance |
| delegation\_responses\[].balance | object | Staked coin for the delegation                |

## Use Cases

* **Portfolio**: Show a user's staking positions
* **Backends**: Typed delegation reads
* **Rewards**: Pair with reward queries

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| InvalidArgument           | Invalid address | The delegator address is invalid                  |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
