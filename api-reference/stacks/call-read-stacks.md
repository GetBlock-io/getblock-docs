---
description: >-
  Example code for the call read REST method. Complete guide on how to use call
  read REST method in GetBlock Web3 documentation.
---

# call read - Stacks

Calls a read-only Clarity function and returns its result, without creating a transaction. Arguments are hex-encoded Clarity values; the result is a hex-encoded Clarity value.

## Endpoint

```http
POST /v2/contracts/call-read/{deployer_address}/{contract_name}/{function_name}
```

## Path Parameters

| Parameter         | Type   | Description                              |
| ----------------- | ------ | ---------------------------------------- |
| deployer\_address | string | The contract deployer's standard address |
| contract\_name    | string | The contract name                        |
| function\_name    | string | The read-only function to call           |

## Example

{% code overflow="wrap" %}
```bash
export STACKS_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${STACKS_RPC}v2/contracts/call-read/SP2J6ZY48GV1EZ5V2V5RB9MP66SW86PYKKNRV9EJ7/my-contract/get-balance" \
  -H "Content-Type: application/json" \
  -d '{"sender": "SP2J6ZY48GV1EZ5V2V5RB9MP66SW86PYKKNRV9EJ7", "arguments": ["0x0516a1b2c3d4e5f60718293a4b5c6d7e8f90a1b2c3"]}'
```
{% endcode %}

## Response

```json
{
  "okay": true,
  "result": "0x010000000000000000000000000000000a"
}
```

## Response Fields

| Field  | Type    | Description                                                         |
| ------ | ------- | ------------------------------------------------------------------- |
| okay   | boolean | Whether the call succeeded                                          |
| result | string  | The returned Clarity value, hex-encoded (present when okay is true) |
| cause  | string  | The failure reason (present when okay is false)                     |

## Use Cases

* **Read state**: Query balances, ownership, or any read-only getter
* **dApp UI**: Populate views without submitting a transaction

## Error Handling

| Error                     | Message       | Description                                                                  |
| ------------------------- | ------------- | ---------------------------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid or the transaction was rejected |
| 404 / Not found           | Not found     | The account, contract, map entry, variable, or constant does not exist       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect                            |
