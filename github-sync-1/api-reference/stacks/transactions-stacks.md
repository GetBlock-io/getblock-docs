---
description: >-
  Example code for the transactions REST method. Complete guide on how to use
  transactions REST method in GetBlock Web3 documentation.
---

# transactions - Stacks

Broadcasts a signed, serialized transaction to the network. The body is the raw transaction bytes (hex string with application/json, or raw bytes with application/octet-stream). On acceptance the node returns the transaction ID.

## Endpoint

```http
POST /v2/transactions
```

## Example

{% code overflow="wrap" %}
```bash
export STACKS_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# The body is the raw, signed, serialized transaction bytes
curl -X POST "${STACKS_RPC}v2/transactions" \
  -H "Content-Type: application/octet-stream" \
  --data-binary @signed-tx.bin
```
{% endcode %}

## Response

```json
"0x7c3b8a2d5f70910013af2c9e4b7a1d6f3028e5c7b9a0d4f1e6c3b8a2d5f70910"
```

## Response Fields

| Field   | Type   | Description                                      |
| ------- | ------ | ------------------------------------------------ |
| (value) | string | The transaction ID (txid) assigned on acceptance |

## Use Cases

* **Transaction submission**: Broadcast a signed STX transfer, contract call, or deploy
* **Confirmation**: Poll the returned txid through an explorer or indexer

## Error Handling

| Error                     | Message       | Description                                                                  |
| ------------------------- | ------------- | ---------------------------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid or the transaction was rejected |
| 404 / Not found           | Not found     | The account, contract, map entry, variable, or constant does not exist       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect                            |
