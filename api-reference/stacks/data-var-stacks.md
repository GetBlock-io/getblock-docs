---
description: >-
  Example code for the data var REST method. Complete guide on how to use data
  var REST method in GetBlock Web3 documentation.
---

# data var - Stacks

Returns the current value of a contract data variable, as a hex-encoded Clarity value, with an optional MARF proof.

## Endpoint

```http
GET /v2/data_var/{principal}/{contract_name}/{var_name}
```

## Path Parameters

| Parameter      | Type   | Description                              |
| -------------- | ------ | ---------------------------------------- |
| principal      | string | The contract deployer's standard address |
| contract\_name | string | The contract name                        |
| var\_name      | string | The data-variable name                   |

## Query Parameters

| Parameter | Type   | Description                            |
| --------- | ------ | -------------------------------------- |
| proof     | number | Set to 0 to omit the MARF Merkle proof |

## Example

{% code overflow="wrap" %}
```bash
export STACKS_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${STACKS_RPC}v2/data_var/SP2J6ZY48GV1EZ5V2V5RB9MP66SW86PYKKNRV9EJ7/my-contract/total-supply?proof=0"
```
{% endcode %}

## Response

```json
{
    "data": "0x010000000000000000000000003b9aca00",
    "proof": "0x0c3a5b6e..."
}
```

## Response Fields

| Field | Type   | Description                               |
| ----- | ------ | ----------------------------------------- |
| data  | string | The variable's value, hex-encoded Clarity |
| proof | string | MARF proof (empty when proof=0)           |

## Use Cases

* **State reads**: Read a contract's mutable data variable
* **Supply tracking**: Read a token's total-supply variable

## Error Handling

| Error                     | Message       | Description                                                                  |
| ------------------------- | ------------- | ---------------------------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid or the transaction was rejected |
| 404 / Not found           | Not found     | The account, contract, map entry, variable, or constant does not exist       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect                            |
