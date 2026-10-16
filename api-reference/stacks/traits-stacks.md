---
description: >-
  Example code for the traits REST method. Complete guide on how to use traits
  REST method in GetBlock Web3 documentation.
---

# traits - Stacks

Reports whether a contract implements a given trait, identified by the trait's deployer, contract, and trait name.

## Endpoint

```http
GET /v2/traits/{deployer_address}/{contract_name}/{trait_deployer_address}/{trait_contract_name}/{trait_name}
```

## Path Parameters

| Parameter                | Type   | Description                          |
| ------------------------ | ------ | ------------------------------------ |
| deployer\_address        | string | The implementing contract's deployer |
| contract\_name           | string | The implementing contract's name     |
| trait\_deployer\_address | string | The trait definition's deployer      |
| trait\_contract\_name    | string | The trait definition's contract      |
| trait\_name              | string | The trait name                       |

## Example

{% code overflow="wrap" %}
```bash
export STACKS_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${STACKS_RPC}v2/traits/SP2J6ZY48GV1EZ5V2V5RB9MP66SW86PYKKNRV9EJ7/my-contract/SP3FBR2AGK5H9QBDH3EEN6DF8EK8JY7RX8QJ5SVTE/sip-010-trait-ft-standard/sip-010-trait"
```
{% endcode %}

## Response

```json
{
    "is_implemented": true
}
```

## Response Fields

| Field           | Type    | Description                                |
| --------------- | ------- | ------------------------------------------ |
| is\_implemented | boolean | Whether the contract conforms to the trait |

## Use Cases

* **Standard checks**: Verify a token contract implements SIP-010
* **Composability**: Confirm trait conformance before a cross-contract call

## Error Handling

| Error                     | Message       | Description                                                                  |
| ------------------------- | ------------- | ---------------------------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid or the transaction was rejected |
| 404 / Not found           | Not found     | The account, contract, map entry, variable, or constant does not exist       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect                            |
