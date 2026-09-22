---
description: >-
  Example code for the cosmos/staking/v1beta1/delegations/{delegator_addr}
  REST method. Complete guide on how to use
  cosmos/staking/v1beta1/delegations/{delegator_addr} REST method in GetBlock
  Web3 documentation.
---

# /cosmos/staking/v1beta1/delegations/{delegator\_addr} - Akash

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

curl "${AKASH_REST}cosmos/staking/v1beta1/delegations/akash17xpfvakm2amg962yls6f84z3kell8c5lazw8j8"
```
{% endcode %}

## Response

```json
{
    "delegation_responses": [],
    "pagination": {
        "next_key": null,
        "total": "0"
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
