---
description: >-
  Example code for the cosmos/tx/v1beta1/txs REST method. Complete guide on how
  to use cosmos/tx/v1beta1/txs REST method in GetBlock Web3 documentation.
---

# /cosmos/tx/v1beta1/txs - Axelar

Submits a signed, protobuf-encoded transaction for inclusion. The broadcast mode selects how long the node waits.

## Endpoint

```http
POST /cosmos/tx/v1beta1/txs
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${AKASH_REST}cosmos/tx/v1beta1/txs" \
--header 'Content-Type: application/json' \
--data-raw '{
    "tx_bytes": "Cr0BCroBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5k...",
    "mode": "BROADCAST_MODE_SYNC"
}'
```
{% endcode %}

## Response

```json
{
    "tx_response": {
        "txhash": "3A1F9C2E7B4D8A05F6C1E3D9B2A4C6E8F0D1B3A5C7E9F2D4B6A8C0E1F3D5B7A9C",
        "code": 0,
        "raw_log": "[]"
    }
}
```

## Response Fields

| Field               | Type    | Description                        |
| ------------------- | ------- | ---------------------------------- |
| tx\_response.txhash | string  | Hash to track the transaction      |
| tx\_response.code   | integer | 0 = accepted; non-zero is an error |

## Use Cases

* **Submission**: Broadcast a signed transaction over REST
* **Mode Control**: Choose sync vs async
* **Wallets**: Submit user-signed transactions

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
