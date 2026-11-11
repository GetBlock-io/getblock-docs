---
description: >-
  Example code for the estimateFee WebSocket method. Complete guide on how to
  use the estimateFee WebSocket method in the GetBlock Web3 documentation.
---

# estimateFee - Bitcoin

Returns the backend fee estimate for one or more confirmation targets. Supplying the transaction's virtual size returns the estimated total fee for a transaction of that size, alongside the per-unit rate. This is the WebSocket form of the REST [api/v2/estimatefee](../bitcoin-blockbook-rest-api/api-v2-estimatefee-bitcoin.md) endpoint.

## Parameters

| Parameter | Type   | Required | Description                                                                   |
| --------- | ------ | -------- | ----------------------------------------------------------------------------- |
| blocks    | array  | Yes      | Confirmation targets in blocks, one estimate returned per entry               |
| specific  | object | No       | Chain-specific options: `conservative` for smart fee mode, `txsize` in vbytes |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>

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
            "feePerTx": "171",
            "feePerUnit": "1190"
        },
        {
            "feePerTx": "74",
            "feePerUnit": "511"
        },
        {
            "feePerTx": "40",
            "feePerUnit": "278"
        }
    ]
}
```

## Response Fields

| Field      | Type   | Description                                                                |
| ---------- | ------ | -------------------------------------------------------------------------- |
| feePerTx   | string | Estimated total fee in satoshis for a transaction of the supplied `txsize` |
| feePerUnit | string | Estimated fee rate in satoshis per **kilobyte**, not per vbyte             |

Results are returned in the same order as the requested `blocks` targets.

{% hint style="warning" %}
`feePerUnit` is quoted per 1000 bytes, following the backend's `estimatesmartfee`, which reports BTC per kilobyte. Dividing by 1000 gives the sat/vB rate wallets usually display: the `1190` above is **1.19 sat/vB**, not 1190. Treating it as sat/vB overpays by three orders of magnitude.

`feePerTx` is derived as `txsize × feePerUnit / 1000`. In the response above, `144 × 1190 / 1000 = 171`.
{% endhint %}

## Use Cases

* **Fee Selection**: Choose a rate matching the confirmation speed a payment needs
* **Cost Preview**: Show the user the fee before signing
* **Tiered Options**: Offer fast, standard, and economy targets from one call
* **Batch Planning**: Size a consolidation against the current rate

{% hint style="info" %}
`feePerTx` is only meaningful when `txsize` is supplied. Without it, size the fee as `vsize × feePerUnit / 1000`, keeping the per-kilobyte denominator in mind.
{% endhint %}

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| error                     | Invalid params | The blocks array is missing or malformed          |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
