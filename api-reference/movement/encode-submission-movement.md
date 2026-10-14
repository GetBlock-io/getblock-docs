---
description: >-
  Example code for the encode submission JSON-RPC method. Complete guide on how
  to use encode submission JSON-RPC in GetBlock Web3 documentation.
---

# encode submission - Movement

Returns the BCS-encoded signing message for a transaction, the bytes a client signs before submitting. Note: for production, encode locally rather than trusting a remote node.

## Endpoint

```http
POST /v1/transactions/encode_submission
```

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${MOVEMENT_REST}v1/transactions/encode_submission" \
  -H "Content-Type: application/json" \
  -d '{"sender": "0xabc...", "sequence_number": "42", "max_gas_amount": "2000", "gas_unit_price": "100", "expiration_timestamp_secs": "1736772600", "payload": {"type": "entry_function_payload", "function": "0x1::coin::transfer", "type_arguments": ["0x1::aptos_coin::AptosCoin"], "arguments": ["0xdef...", "1000000000"]}}'
```
{% endcode %}

## Response

```json
"0xb5e97db07fa0bd0e5598aa3643a9bc6f6693bddc1a9fec9e674a461eaa00b193ade..."
```

## Response Fields

| Field  | Type   | Description                                  |
| ------ | ------ | -------------------------------------------- |
| (root) | string | Hex-encoded BCS signing message to be signed |

## Use Cases

* **Transaction Building**: Produce the signing message from transaction JSON
* **Offline Signing**: Pair with a local signer

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
