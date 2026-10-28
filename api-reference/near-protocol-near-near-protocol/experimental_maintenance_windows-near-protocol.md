---
description: >-
  Example code for the EXPERIMENTAL_maintenance_windows JSON-RPC method.
  Complete guide on how to use EXPERIMENTAL_maintenance_windows JSON-RPC in
  GetBlock Web3 documentation.
---

# EXPERIMENTAL\_maintenance\_windows - NEAR Protocol

Returns the upcoming maintenance windows (block-height ranges) for a validator account — the periods in the current epoch when it is neither producing nor validating and can safely restart.

## Method

```
EXPERIMENTAL_maintenance_windows
```

## Parameters

The `params` object carries:

| Parameter   | Example                 | Description |
| ----------- | ----------------------- | ----------- |
| account\_id | `"figment.poolv1.near"` | —           |

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "EXPERIMENTAL_maintenance_windows", "params": {"account_id": "figment.poolv1.near"}}'
```
{% endcode %}

## Response

```json
[
    [
        152000100,
        152000130
    ],
    [
        152000400,
        152000455
    ]
]
```

## Response Fields

| Field   | Type  | Description                                                                        |
| ------- | ----- | ---------------------------------------------------------------------------------- |
| (array) | array | A list of \[start\_height, end\_height] windows during which the validator is idle |

## Use Cases

* **Validator ops**: Schedule node restarts during idle windows
* **Automation**: Drive maintenance tooling for a staking pool

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
