# /cosmos/tx/v1beta1/txs- Cronos

Submits a signed, protobuf-encoded transaction for inclusion. The broadcast mode selects how long the node waits: BROADCAST\_MODE\_SYNC returns after CheckTx, ASYNC returns immediately.

## Endpoint

```http
POST /cosmos/tx/v1beta1/txs
```

## Example

{% code overflow="wrap" %}
```bash
export CRONOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${CRONOS_REST}cosmos/tx/v1beta1/txs" \
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
        "height": "0",
        "txhash": "A1B2C3D4E5F60718293A4B5C6D7E8F90112233445566778899AABBCCDDEEFF00",
        "code": 0,
        "raw_log": "[]",
        "gas_wanted": "200000",
        "gas_used": "0"
    }
}
```

## Response Fields

| Field                 | Type    | Description                                   |
| --------------------- | ------- | --------------------------------------------- |
| tx\_response.txhash   | string  | Hash to track the transaction                 |
| tx\_response.code     | integer | 0 means accepted; non-zero is a CheckTx error |
| tx\_response.raw\_log | string  | Log, populated on rejection                   |

## Use Cases

* **Transaction Submission**: Broadcast a signed Cosmos transaction over REST
* **Mode Control**: Choose sync vs async broadcast behaviour
* **Wallet Backends**: Submit user-signed transactions

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / decode error        | Decode error  | tx\_bytes could not be decoded                    |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
