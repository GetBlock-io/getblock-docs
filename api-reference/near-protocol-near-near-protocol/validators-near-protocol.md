---
description: >-
  Example code for the validators JSON-RPC method. Complete guide on how to use
  validators JSON-RPC in GetBlock Web3 documentation.
---

# validators - NEAR Protocol

Returns the active validator set and validation state for an epoch. Pass \[null] for the latest block, or a block height, block hash, or epoch id.

## Method

```
validators
```

## Parameters

Takes an empty array `[]` (or `[null]` for the latest block) — see the example.

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "validators", "params": [null]}'
```
{% endcode %}

## Response

```json
{
    "current_validators": [
        {
            "account_id": "figment.poolv1.near",
            "public_key": "ed25519:DcA2MzgpJbrUATQLLceocVckhhAqrkingax4oJ9kZ847",
            "stake": "35000000000000000000000000000",
            "num_produced_blocks": 120,
            "num_expected_blocks": 120
        }
    ],
    "next_validators": [],
    "current_proposals": [],
    "epoch_start_height": 152000000
}
```

## Response Fields

| Field                | Type   | Description                                                          |
| -------------------- | ------ | -------------------------------------------------------------------- |
| current\_validators  | array  | Validators active this epoch, with stake and block-production counts |
| next\_validators     | array  | Validators for the next epoch                                        |
| current\_proposals   | array  | Pending validator proposals                                          |
| epoch\_start\_height | number | Height at which the current epoch began                              |

## Use Cases

* **Staking UI**: List active validators and their stake
* **Performance**: Compare produced vs expected blocks per validator

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
