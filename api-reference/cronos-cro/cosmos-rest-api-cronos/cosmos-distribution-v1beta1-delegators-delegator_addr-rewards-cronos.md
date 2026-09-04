# /cosmos/distribution/v1beta1/delegators/{delegator\_addr}/rewards - Cronos

Returns the total outstanding staking rewards for a delegator across all validators, plus a per-validator breakdown.

## Endpoint

```http
GET /cosmos/distribution/v1beta1/delegators/{delegator_addr}/rewards
```

## Path Parameters

| Parameter       | Type   | Description                      |
| --------------- | ------ | -------------------------------- |
| delegator\_addr | string | Delegator bech32 address (crc1…) |

## Example

{% code overflow="wrap" %}
```bash
export CRONOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${CRONOS_REST}cosmos/distribution/v1beta1/delegators/crc1p9zq0lr6h6n5s2wk9m3v7t4x8c6b2d0f1g3h5j/rewards"
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

| Field   | Type  | Description                                     |
| ------- | ----- | ----------------------------------------------- |
| rewards | array | Per-validator reward coins for the delegator    |
| total   | array | Total accumulated rewards across all validators |

## Use Cases

* **Reward Display**: Show a delegator's claimable rewards
* **Claim Flows**: Decide whether a claim is worthwhile
* **Accounting**: Track reward accrual

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| 400 / invalid address     | Invalid address | The delegator address is not valid bech32         |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
