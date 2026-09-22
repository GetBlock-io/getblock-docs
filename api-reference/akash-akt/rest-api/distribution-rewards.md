---
description: >-
  Example code for the
  cosmos/distribution/v1beta1/delegators/{delegator_addr}/rewards REST method.
  Complete guide on how to use
  cosmos/distribution/v1beta1/delegators/{delegator_addr}/rewards REST method
  in GetBlock Web3 documentation.
---

# /cosmos/distribution/v1beta1/delegators/{delegator\_addr}/rewards - Akash

Returns the total outstanding staking rewards for a delegator across all validators, with a per-validator breakdown.

## Endpoint

```http
GET /cosmos/distribution/v1beta1/delegators/{delegator_addr}/rewards
```

## Path Parameters

| Parameter       | Type   | Description                        |
| --------------- | ------ | ---------------------------------- |
| delegator\_addr | string | Delegator bech32 address (akash1…) |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/distribution/v1beta1/delegators/akash17xpfvakm2amg962yls6f84z3kell8c5lazw8j8/rewards"
```
{% endcode %}

## Response

```json
{
    "rewards": [],
    "total": []
}
```

## Response Fields

| Field   | Type  | Description               |
| ------- | ----- | ------------------------- |
| rewards | array | Per-validator rewards     |
| total   | array | Total accumulated rewards |

## Use Cases

* **Reward Display**: Show claimable rewards
* **Claim Flows**: Decide when to claim
* **Accounting**: Track accrual

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
