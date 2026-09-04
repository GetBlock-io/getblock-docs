---
description: >-
  Example code for the cosmos.distribution.v1beta1.Query/DelegationTotalRewards
  gRPC method. Complete guide on how to use
  cosmos.distribution.v1beta1.Query/DelegationTotalRewards gRPC method in
  GetBlock Web3 documentation.
---

# cosmos.distribution.v1beta1.Query/DelegationTotalRewards - Cronos

Returns the total outstanding staking rewards for a delegator across all validators, with a per-validator breakdown. The gRPC equivalent of the REST rewards query.

## Service Method

```
cosmos.distribution.v1beta1.Query/DelegationTotalRewards
```

## Message Fields

| Field              | Type   | Description                      |
| ------------------ | ------ | -------------------------------- |
| delegator\_address | string | Delegator bech32 address (crc1…) |

## Example

{% code overflow="wrap" %}
```bash
grpcurl \
  -H "x-api-key: <ACCESS-TOKEN>" \
  -d '{
    "delegator_address": "crc1p9zq0lr6h6n5s2wk9m3v7t4x8c6b2d0f1g3h5j"
}' \
  shared.eu-central-1.getblock.io:443 \
  cosmos.distribution.v1beta1.Query/DelegationTotalRewards
```
{% endcode %}

## Response

```json
{
    "rewards": [
        {
            "validator_address": "crcvaloper1p9zq0lr6h6n5s2wk9m3v7t4x8c6b2d0f1g3h5j",
            "reward": [
                {
                    "denom": "basecro",
                    "amount": "1234567890.000000000000000000"
                }
            ]
        }
    ],
    "total": [
        {
            "denom": "basecro",
            "amount": "1234567890.000000000000000000"
        }
    ]
}
```

## Response Fields

| Field   | Type  | Description                     |
| ------- | ----- | ------------------------------- |
| rewards | array | Per-validator rewards           |
| total   | array | Total rewards across validators |

## Use Cases

* **Reward Display**: Show claimable rewards
* **Backends**: Typed reward reads
* **Claim Flows**: Decide when to claim

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| InvalidArgument           | Invalid address | The delegator address is invalid                  |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
