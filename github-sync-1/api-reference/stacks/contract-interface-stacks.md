---
description: >-
  Example code for the contract interface REST method. Complete guide on how to
  use contract interface REST method in GetBlock Web3 documentation.
---

# contract interface - Stacks

Returns the interface (ABI) of a deployed Clarity contract: its public and read-only functions, data variables, maps, fungible and non-fungible tokens, and the Clarity version.

## Endpoint

```http
GET /v2/contracts/interface/{deployer_address}/{contract_name}
```

## Path Parameters

| Parameter         | Type   | Description                              |
| ----------------- | ------ | ---------------------------------------- |
| deployer\_address | string | The contract deployer's standard address |
| contract\_name    | string | The contract name                        |

## Example

{% code overflow="wrap" %}
```bash
export STACKS_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${STACKS_RPC}v2/contracts/interface/SP2J6ZY48GV1EZ5V2V5RB9MP66SW86PYKKNRV9EJ7/my-contract"
```
{% endcode %}

## Response

```json
{
    "functions": [
        {
            "name": "transfer",
            "access": "public",
            "args": [
                {
                    "name": "amount",
                    "type": "uint128"
                },
                {
                    "name": "recipient",
                    "type": "principal"
                }
            ],
            "outputs": {
                "type": {
                    "response": {
                        "ok": "bool",
                        "error": "uint128"
                    }
                }
            }
        }
    ],
    "variables": [],
    "maps": [
        {
            "name": "balances",
            "key": "principal",
            "value": "uint128"
        }
    ],
    "fungible_tokens": [],
    "non_fungible_tokens": [],
    "clarity_version": "Clarity3",
    "epoch": "Epoch30"
}
```

## Response Fields

| Field                 | Type   | Description                                                   |
| --------------------- | ------ | ------------------------------------------------------------- |
| functions             | array  | The contract's functions, each with access, args, and outputs |
| variables             | array  | Data variables declared in the contract                       |
| maps                  | array  | Data maps, each with key and value types                      |
| fungible\_tokens      | array  | Fungible tokens defined by the contract                       |
| non\_fungible\_tokens | array  | Non-fungible tokens defined by the contract                   |
| clarity\_version      | string | The contract's Clarity version                                |

## Use Cases

* **Call building**: Read a contract's function signatures before calling
* **Type discovery**: Inspect maps and token definitions

## Error Handling

| Error                     | Message       | Description                                                                  |
| ------------------------- | ------------- | ---------------------------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid or the transaction was rejected |
| 404 / Not found           | Not found     | The account, contract, map entry, variable, or constant does not exist       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect                            |
