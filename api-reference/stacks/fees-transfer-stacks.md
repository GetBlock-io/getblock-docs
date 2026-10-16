---
description: >-
  Example code for the fees transfer REST method. Complete guide on how to use
  fees transfer REST method in GetBlock Web3 documentation.
---

# fees transfer - Stacks

Returns the estimated fee rate for a standard STX token-transfer transaction, in microSTX per byte.

## Endpoint

```http
GET /v2/fees/transfer
```

## Example

{% code overflow="wrap" %}
```bash
export STACKS_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${STACKS_RPC}v2/fees/transfer"
```
{% endcode %}

## Response

```json
1
```

## Response Fields

| Field   | Type   | Description                                                  |
| ------- | ------ | ------------------------------------------------------------ |
| (value) | number | Estimated fee rate in microSTX per byte for a token transfer |

## Use Cases

* **Fee estimation**: Price a simple STX transfer before signing
* **Baseline**: Use as a floor for transfer fee calculations

## Error Handling

| Error                     | Message       | Description                                                                  |
| ------------------------- | ------------- | ---------------------------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid or the transaction was rejected |
| 404 / Not found           | Not found     | The account, contract, map entry, variable, or constant does not exist       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect                            |
