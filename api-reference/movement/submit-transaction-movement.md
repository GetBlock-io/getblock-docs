---
description: >-
  Example code for the submit transaction JSON-RPC method. Complete guide on how
  to use submit transaction JSON-RPC in GetBlock Web3 documentation.
---

# submit transaction - Movement

Submits a signed transaction for execution and returns the pending transaction (including its hash). The body is a signed transaction in JSON (or BCS when the Content-Type is `application/x.aptos.signed_transaction+bcs`).

## Endpoint

```http
POST /v1/transactions
```

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${MOVEMENT_REST}v1/transactions" \
  -H "Content-Type: application/json" \
  -d '{"sender": "0xabc...", "sequence_number": "42", "max_gas_amount": "2000", "gas_unit_price": "100", "expiration_timestamp_secs": "1736772600", "payload": {"type": "entry_function_payload", "function": "0x1::coin::transfer", "type_arguments": ["0x1::aptos_coin::AptosCoin"], "arguments": ["0xdef...", "1000000000"]}, "signature": {"type": "ed25519_signature", "public_key": "0x...", "signature": "0x..."}}'
```
{% endcode %}

## Response

```json
{
    "hash": "0xade...",
    "sender": "0xabc...",
    "sequence_number": "42",
    "payload": {
        "type": "entry_function_payload",
        "function": "0x1::coin::transfer"
    }
}
```

## Response Fields

| Field | Type   | Description                                 |
| ----- | ------ | ------------------------------------------- |
| hash  | string | Hash of the submitted (pending) transaction |

## Use Cases

* **Transaction Submission**: Broadcast a signed Move transaction
* **dApp Writes**: Transfer MOVE or call entry functions

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
