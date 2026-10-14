---
description: >-
  Example code for the simulate transaction JSON-RPC method. Complete guide on
  how to use simulate transaction JSON-RPC in GetBlock Web3 documentation.
---

# simulate transaction - Movement

Simulates a transaction without committing it and returns its predicted effects, gas usage, and success status. The signature is not verified (a zero signature is used), so it is ideal for gas estimation.

## Endpoint

```http
POST /v1/transactions/simulate
```

## Query Parameters

| Parameter                  | Type   | Description                                           |
| -------------------------- | ------ | ----------------------------------------------------- |
| estimate\_gas\_unit\_price | string | Let the node estimate the gas unit price (true/false) |
| estimate\_max\_gas\_amount | string | Let the node estimate the max gas (true/false)        |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${MOVEMENT_REST}v1/transactions/simulate?estimate_gas_unit_price=true" \
  -H "Content-Type: application/json" \
  -d '{"sender": "0xabc...", "sequence_number": "42", "max_gas_amount": "2000", "gas_unit_price": "100", "expiration_timestamp_secs": "1736772600", "payload": {"type": "entry_function_payload", "function": "0x1::coin::transfer", "type_arguments": ["0x1::aptos_coin::AptosCoin"], "arguments": ["0xdef...", "1000000000"]}, "signature": {"type": "ed25519_signature", "public_key": "0x...", "signature": "0x00"}}'
```
{% endcode %}

## Response

```json
[
    {
        "version": "0",
        "hash": "0x0",
        "success": true,
        "vm_status": "Executed successfully",
        "gas_used": "7",
        "gas_unit_price": "100"
    }
]
```

## Response Fields

| Field      | Type    | Description                           |
| ---------- | ------- | ------------------------------------- |
| success    | boolean | Whether the transaction would succeed |
| gas\_used  | string  | Predicted gas usage                   |
| vm\_status | string  | Predicted VM status                   |

## Use Cases

* **Gas Estimation**: Size gas before signing
* **Validation**: Catch failures before broadcasting

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
