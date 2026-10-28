---
description: >-
  Example code for the EXPERIMENTAL_receipt JSON-RPC method. Complete guide on
  how to use EXPERIMENTAL_receipt JSON-RPC in GetBlock Web3 documentation.
---

# EXPERIMENTAL\_receipt - NEAR Protocol

Fetches a single receipt by its id, returning its predecessor, receiver, and action or data payload.

## Method

```
EXPERIMENTAL_receipt
```

## Parameters

The `params` object carries:

| Parameter   | Example               | Description |
| ----------- | --------------------- | ----------- |
| receipt\_id | `"2jZ...receipthash"` | —           |

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "EXPERIMENTAL_receipt", "params": {"receipt_id": "2jZ...receipthash"}}'
```
{% endcode %}

## Response

```json
{
    "predecessor_id": "example.near",
    "receiver_id": "token.near",
    "receipt_id": "2jZ...",
    "receipt": {
        "Action": {
            "signer_id": "example.near",
            "actions": [
                {
                    "FunctionCall": {
                        "method_name": "ft_transfer"
                    }
                }
            ]
        }
    }
}
```

## Response Fields

| Field           | Type   | Description                           |
| --------------- | ------ | ------------------------------------- |
| predecessor\_id | string | The account that produced the receipt |
| receiver\_id    | string | The account the receipt is applied to |
| receipt         | object | The receipt's action or data payload  |

## Use Cases

* **Receipt inspection**: Resolve a specific receipt referenced in a transaction outcome

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
