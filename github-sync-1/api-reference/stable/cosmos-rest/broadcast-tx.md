---
description: >-
  Example code for the broadcast tx REST method. Complete guide on how to use
  broadcast tx REST method in GetBlock Web3 documentation.
---

# broadcast tx - Stable

Broadcasts a signed, protobuf-encoded transaction. The body carries the base64 tx\_bytes and a broadcast mode (SYNC, ASYNC, or BLOCK).

## Endpoint

```http
POST /cosmos/tx/v1beta1/txs
```

## Example

{% code overflow="wrap" %}
```bash
export STABLE_REST=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${STABLE_REST}cosmos/tx/v1beta1/txs" \
  -H "Content-Type: application/json" \
  -d '{"tx_bytes": "Cr4BCr...base64-encoded-tx", "mode": "BROADCAST_MODE_SYNC"}'
```
{% endcode %}

## Response

```json
{
    "tx_response": {
        "height": "0",
        "txhash": "9A1B2C...",
        "code": 0,
        "raw_log": "",
        "gas_wanted": "200000",
        "gas_used": "84000"
    }
}
```

## Response Fields

| Field                  | Type   | Description                |
| ---------------------- | ------ | -------------------------- |
| tx\_response.txhash    | string | The transaction hash       |
| tx\_response.code      | number | Result code (0 = accepted) |
| tx\_response.raw\_log  | string | Log or error message       |
| tx\_response.gas\_used | string | Gas used                   |

## Use Cases

* **Submission**: Broadcast a signed Cosmos transaction
* **Mode choice**: Use SYNC for mempool confirmation, ASYNC for fire-and-forget

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
