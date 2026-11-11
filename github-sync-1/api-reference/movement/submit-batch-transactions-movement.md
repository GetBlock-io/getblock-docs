---
description: >-
  Example code for the submit batch transactions JSON-RPC method. Complete guide
  on how to use submit batch transactions JSON-RPC in GetBlock Web3
  documentation.
---

# submit batch transactions - Movement

Submits a batch of signed transactions in one call. Returns any transactions that failed submission; an empty failure list means all were accepted.

## Endpoint

```http
POST /v1/transactions/batch
```

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${MOVEMENT_REST}v1/transactions/batch" \
  -H "Content-Type: application/json" \
  -d '[{"sender": "0xabc...", "sequence_number": "42", "max_gas_amount": "2000", "gas_unit_price": "100", "expiration_timestamp_secs": "1736772600", "payload": {"type": "entry_function_payload", "function": "0x1::coin::transfer", "type_arguments": ["0x1::aptos_coin::AptosCoin"], "arguments": ["0xdef...", "1000000000"]}, "signature": {"type": "ed25519_signature", "public_key": "0x...", "signature": "0x..."}}]'
```
{% endcode %}

## Response

```json
{
    "transaction_failures": []
}
```

## Response Fields

| Field                 | Type  | Description                                                   |
| --------------------- | ----- | ------------------------------------------------------------- |
| transaction\_failures | array | Per-transaction submission failures; empty when all succeeded |

## Use Cases

* **Throughput**: Submit many signed transactions in one request

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
