---
description: >-
  Example code for the cosmos/base/tendermint/v1beta1/validatorsets/latest
  REST method. Complete guide on how to use
  cosmos/base/tendermint/v1beta1/validatorsets/latest REST method in GetBlock
  Web3 documentation.
---

# /cosmos/base/tendermint/v1beta1/validatorsets/latest - Akash

Returns the latest validator set via the Cosmos base service.

## Endpoint

```
GET /cosmos/base/tendermint/v1beta1/validatorsets/latest
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/base/tendermint/v1beta1/validatorsets/latest"
```
{% endcode %}

## Response

```json
{
    "block_height": "19500000",
    "validators": [
        {
            "address": "akashvalcons1...",
            "voting_power": "5000000"
        }
    ],
    "pagination": {
        "total": "100"
    }
}
```

## Response Fields

| Field         | Type   | Description                            |
| ------------- | ------ | -------------------------------------- |
| validators    | array  | Consensus validators with voting power |
| block\_height | string | Height of the set                      |

## Use Cases

* **Consensus**: Read the validator set over REST

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
