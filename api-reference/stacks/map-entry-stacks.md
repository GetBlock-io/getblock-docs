---
description: >-
  Example code for the map entry REST method. Complete guide on how to use map
  entry REST method in GetBlock Web3 documentation.
---

# map entry - Stacks

Returns the value stored at a key in a contract's data map. The request body is the hex-encoded Clarity value of the key; the response is the hex-encoded Clarity value and an optional MARF proof.

## Endpoint

```
POST /v2/map_entry/{deployer_address}/{contract_name}/{map_name}
```

## Path Parameters

| Parameter         | Type   | Description                              |
| ----------------- | ------ | ---------------------------------------- |
| deployer\_address | string | The contract deployer's standard address |
| contract\_name    | string | The contract name                        |
| map\_name         | string | The data-map name                        |

## Query Parameters

| Parameter | Type   | Description                            |
| --------- | ------ | -------------------------------------- |
| proof     | number | Set to 0 to omit the MARF Merkle proof |

## Example

{% code overflow="wrap" %}
```bash
export STACKS_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${STACKS_RPC}v2/map_entry/SP2J6ZY48GV1EZ5V2V5RB9MP66SW86PYKKNRV9EJ7/my-contract/balances" \
  -H "Content-Type: application/json" \
  -d '"0x0516a1b2c3d4e5f60718293a4b5c6d7e8f90a1b2c3"'
```
{% endcode %}

## Response

```json
{
    "data": "0x0a0100000000000000000000000000000064",
    "proof": "0x0c3a5b6e..."
}
```

## Response Fields

| Field | Type   | Description                                                                 |
| ----- | ------ | --------------------------------------------------------------------------- |
| data  | string | The map value as a hex-encoded Clarity optional (none if the key is absent) |
| proof | string | MARF proof (empty when proof=0)                                             |

## Use Cases

* **Map reads**: Fetch a specific entry from a contract's data map
* **Indexing**: Resolve per-key state without a read-only call

## Error Handling

| Error                     | Message       | Description                                                                  |
| ------------------------- | ------------- | ---------------------------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid or the transaction was rejected |
| 404 / Not found           | Not found     | The account, contract, map entry, variable, or constant does not exist       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect                            |
