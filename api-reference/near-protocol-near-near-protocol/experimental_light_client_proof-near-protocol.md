---
description: >-
  Example code for the EXPERIMENTAL_light_client_proof JSON-RPC method. Complete
  guide on how to use EXPERIMENTAL_light_client_proof JSON-RPC in GetBlock Web3
  documentation.
---

# EXPERIMENTAL\_light\_client\_proof - NEAR Protocol

Returns an execution-outcome inclusion proof (for a transaction or receipt) against a given light-client head, so a light client can verify the outcome.

## Method

```
EXPERIMENTAL_light_client_proof
```

## Parameters

The `params` object carries:

| Parameter           | Example          | Description |
| ------------------- | ---------------- | ----------- |
| type                | `"transaction"`  | —           |
| transaction\_hash   | `"B2d..."`       | —           |
| sender\_id          | `"example.near"` | —           |
| light\_client\_head | `"9MzuZ..."`     | —           |

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "EXPERIMENTAL_light_client_proof", "params": {"type": "transaction", "transaction_hash": "B2d...", "sender_id": "example.near", "light_client_head": "9MzuZ..."}}'
```
{% endcode %}

## Response

```json
{
    "outcome_proof": {
        "id": "B2d...",
        "outcome": {
            "status": {
                "SuccessValue": ""
            }
        }
    },
    "outcome_root_proof": [],
    "block_header_lite": {
        "inner_lite": {
            "height": 152000000
        }
    },
    "block_proof": []
}
```

## Response Fields

| Field                | Type   | Description                                           |
| -------------------- | ------ | ----------------------------------------------------- |
| outcome\_proof       | object | The Merkle proof of the transaction/receipt outcome   |
| outcome\_root\_proof | array  | Proof linking the outcome to the block's outcome root |
| block\_header\_lite  | object | The compact header the proof is anchored to           |
| block\_proof         | array  | Proof linking the block to the light-client head      |

## Use Cases

* **Trustless verification**: Prove a transaction executed, to a light client or bridge

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
