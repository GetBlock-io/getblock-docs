# cosmos.tx.v1beta1.Service/BroadcastTx - Cronos

Submits a signed, protobuf-encoded transaction and returns the broadcast result. The gRPC equivalent of the REST broadcast endpoint; the mode selects sync vs async behaviour.

## Service Method

```
cosmos.tx.v1beta1.Service/BroadcastTx
```

## Message Fields

| Field     | Type   | Description                                     |
| --------- | ------ | ----------------------------------------------- |
| tx\_bytes | string | Base64/hex protobuf-encoded signed transaction  |
| mode      | string | BROADCAST\_MODE\_SYNC or BROADCAST\_MODE\_ASYNC |

## Example

{% code overflow="wrap" %}
```bash
grpcurl \
  -H "x-api-key: <ACCESS-TOKEN>" \
  -d '{
    "tx_bytes": "Cr0BCroBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5k...",
    "mode": "BROADCAST_MODE_SYNC"
}' \
  shared.eu-central-1.getblock.io:443 \
  cosmos.tx.v1beta1.Service/BroadcastTx
```
{% endcode %}

## Response

```json
{
  "tx_response": {
    "txhash": "A1B2C3D4E5F60718293A4B5C6D7E8F90112233445566778899AABBCCDDEEFF00",
    "code": 0,
    "raw_log": "[]",
    "gas_wanted": "200000",
    "gas_used": "0"
  }
}
```

## Response Fields

| Field               | Type    | Description                            |
| ------------------- | ------- | -------------------------------------- |
| tx\_response.txhash | string  | Hash to track the transaction          |
| tx\_response.code   | integer | 0 means accepted; non-zero is an error |

## Use Cases

* **Transaction Submission**: Broadcast a signed transaction over gRPC
* **Backends**: Typed broadcast in a production service
* **Mode Control**: Choose sync vs async

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| InvalidArgument           | Decode error  | tx\_bytes could not be decoded                    |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
