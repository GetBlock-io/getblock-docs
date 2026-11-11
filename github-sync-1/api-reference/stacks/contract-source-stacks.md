---
description: >-
  Example code for the contract source REST method. Complete guide on how to use
  contract source REST method in GetBlock Web3 documentation.
---

# contract source - Stacks

Returns the Clarity source code of a deployed contract, the block height at which it was published, and optionally a MARF proof.

## Endpoint

```http
GET /v2/contracts/source/{deployer_address}/{contract_name}
```

## Path Parameters

| Parameter         | Type   | Description                              |
| ----------------- | ------ | ---------------------------------------- |
| deployer\_address | string | The contract deployer's standard address |
| contract\_name    | string | The contract name                        |

## Query Parameters

| Parameter | Type   | Description                            |
| --------- | ------ | -------------------------------------- |
| proof     | number | Set to 0 to omit the MARF Merkle proof |

## Example

{% code overflow="wrap" %}
```bash
export STACKS_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${STACKS_RPC}v2/contracts/source/SP2J6ZY48GV1EZ5V2V5RB9MP66SW86PYKKNRV9EJ7/my-contract?proof=0"
```
{% endcode %}

## Response

```json
{
    "source": "(define-public (transfer (amount uint) (recipient principal))\n  (ok true))",
    "publish_height": 450120,
    "proof": "0x0c3a5b6e..."
}
```

## Response Fields

| Field           | Type   | Description                                             |
| --------------- | ------ | ------------------------------------------------------- |
| source          | string | The contract's Clarity source code                      |
| publish\_height | number | Stacks block height at which the contract was published |
| proof           | string | MARF proof (empty when proof=0)                         |

## Use Cases

* **Verification**: Inspect the deployed Clarity source of a contract
* **Tooling**: Feed source into a Clarity analyzer or explorer

## Error Handling

| Error                     | Message       | Description                                                                  |
| ------------------------- | ------------- | ---------------------------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid or the transaction was rejected |
| 404 / Not found           | Not found     | The account, contract, map entry, variable, or constant does not exist       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect                            |
