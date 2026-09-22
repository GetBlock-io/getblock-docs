---
description: >-
  Example code for the cosmos/tx/v1beta1/txs/{hash} REST method. Complete
  guide on how to use cosmos/tx/v1beta1/txs/{hash} REST method in GetBlock
  Web3 documentation.
---

# /cosmos/tx/v1beta1/txs/{hash} - Akash

Returns a decoded transaction and its response by hash via the Cosmos tx service.

## Endpoint

```
GET /cosmos/tx/v1beta1/txs/{hash}
```

## Path Parameters

| Parameter | Type   | Description      |
| --------- | ------ | ---------------- |
| hash      | string | Transaction hash |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/tx/v1beta1/txs/3A1F9C2E7B4D8A05F6C1E3D9B2A4C6E8F0D1B3A5C7E9F2D4B6A8C0E1F3D5B7A9C"
```
{% endcode %}

## Response

```json
{
    "tx": {
        "body": {
            "messages": []
        }
    },
    "tx_response": {
        "txhash": "3A1F9C2E7B4D8A05F6C1E3D9B2A4C6E8F0D1B3A5C7E9F2D4B6A8C0E1F3D5B7A9C",
        "height": "19500000",
        "code": 0,
        "gas_used": "118000"
    }
}
```

## Response Fields

| Field        | Type   | Description                            |
| ------------ | ------ | -------------------------------------- |
| tx           | object | Decoded transaction                    |
| tx\_response | object | Execution response (height, code, gas) |

## Use Cases

* **Receipts**: Fetch a tx by hash over REST

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
