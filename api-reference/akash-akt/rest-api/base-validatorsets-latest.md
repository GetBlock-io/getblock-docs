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

curl "${AKASH_REST}cosmos/base/tendermint/v1beta1/validatorsets/latest?pagination.limit=2"
```
{% endcode %}

## Response

```json
{
    "block_height": "28742382",
    "validators": [
        {
            "address": "akashvalcons1kxzj69l6v66ns250wurjtnzmy29n2a6s68zns2",
            "pub_key": {
                "@type": "/cosmos.crypto.ed25519.PubKey",
                "key": "7AaTbVWTaspcBBsJHHoxGx8wZb0rZbYL4l7QkQT+uPM="
            },
            "voting_power": "10744784",
            "proposer_priority": "-13780471"
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "83"
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
