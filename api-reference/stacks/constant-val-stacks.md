---
description: >-
  Example code for the constant val REST method. Complete guide on how to use
  constant val REST method in GetBlock Web3 documentation.
---

# constant val - Stacks

Returns the value of a constant defined in a contract, as a hex-encoded Clarity value.

## Endpoint

```http
GET /v2/constant_val/{deployer_address}/{contract_name}/{constant_name}
```

## Path Parameters

| Parameter         | Type   | Description                              |
| ----------------- | ------ | ---------------------------------------- |
| deployer\_address | string | The contract deployer's standard address |
| contract\_name    | string | The contract name                        |
| constant\_name    | string | The constant name                        |

## Example

{% code overflow="wrap" %}
```bash
export STACKS_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${STACKS_RPC}v2/constant_val/SP2J6ZY48GV1EZ5V2V5RB9MP66SW86PYKKNRV9EJ7/my-contract/ERR-NOT-AUTHORIZED"
```
{% endcode %}

## Response

```json
{
    "data": "0x080000000000000000000000000000000d"
}
```

## Response Fields

| Field | Type   | Description                               |
| ----- | ------ | ----------------------------------------- |
| data  | string | The constant's value, hex-encoded Clarity |

## Use Cases

* **Error decoding**: Resolve a contract's error-code constants
* **Config reads**: Inspect fixed parameters baked into a contract

## Error Handling

| Error                     | Message       | Description                                                                  |
| ------------------------- | ------------- | ---------------------------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid or the transaction was rejected |
| 404 / Not found           | Not found     | The account, contract, map entry, variable, or constant does not exist       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect                            |
