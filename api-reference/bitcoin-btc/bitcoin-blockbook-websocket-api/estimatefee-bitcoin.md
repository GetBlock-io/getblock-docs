---
description: >-
  Example code for the estimateFee WebSocket method. Complete guide on how to
  use the estimateFee WebSocket method in the GetBlock Web3 documentation.
---

# estimateFee - Bitcoin

Returns the backend fee estimate for one or more confirmation targets. Supplying the transaction's virtual size returns the estimated total fee for a transaction of that size, alongside the per-unit rate. This is the WebSocket form of the REST [api/v2/estimatefee](../bitcoin-blockbook-rest-api/api-v2-estimatefee-bitcoin.md) endpoint.

## Parameters

| Parameter | Type   | Required | Description                                                              |
| --------- | ------ | -------- | -------------------------------------------------------------------------- |
| blocks    | array  | Yes      | Confirmation targets in blocks, one estimate returned per entry           |
| specific  | object | No       | Chain-specific options: `conservative` for smart fee mode, `txsize` in vbytes |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/websocket

# then send:
{
    "id": "getblock.io",
    "method": "estimateFee",
    "params": {
        "blocks": [1, 6, 24],
        "specific": {
            "conservative": true,
            "txsize": 144
        }
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": [
        {
            "feePerTx": "3542",
            "feePerUnit": "24"
        },
        {
            "feePerTx": "1584",
            "feePerUnit": "11"
        },
        {
            "feePerTx": "864",
            "feePerUnit": "6"
        }
    ]
}
```

## Response Fields

| Field      | Type   | Description                                                                  |
| ---------- | ------ | ------------------------------------------------------------------------------ |
| feePerTx   | string | Estimated total fee in satoshis for a transaction of the supplied `txsize`    |
| feePerUnit | string | Estimated fee rate in satoshis per vbyte                                      |

Results are returned in the same order as the requested `blocks` targets.

## Use Cases

* **Fee Selection**: Choose a rate matching the confirmation speed a payment needs
* **Cost Preview**: Show the user the fee before signing
* **Tiered Options**: Offer fast, standard, and economy targets from one call
* **Batch Planning**: Size a consolidation against the current rate

{% hint style="info" %}
`feePerTx` is only meaningful when `txsize` is supplied. Without it, size the fee from `feePerUnit` multiplied by the transaction's virtual size in vbytes.
{% endhint %}

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| error                     | Invalid params | The blocks array is missing or malformed         |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
