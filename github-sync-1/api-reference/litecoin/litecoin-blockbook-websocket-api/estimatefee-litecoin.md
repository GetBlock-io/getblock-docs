---
description: >-
  Example code for the estimateFee WebSocket method. Complete guide on how to
  use the estimateFee WebSocket method in the GetBlock Web3 documentation.
---

# estimateFee - Litecoin

Returns the backend fee estimate for one or more confirmation targets. Supplying the transaction's virtual size returns the estimated total fee alongside the per-unit rate.

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
        "blocks": [1, 6, 12],
        "specific": {
            "conservative": true,
            "txsize": 225
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
            "feePerTx": "224",
            "feePerUnit": "994"
        },
        {
            "feePerTx": "224",
            "feePerUnit": "994"
        },
        {
            "feePerTx": "224",
            "feePerUnit": "994"
        }
    ]
}
```

Results are returned in the same order as the requested `blocks` targets.

## Response Fields

| Field      | Type   | Description                                                                |
| ---------- | ------ | -------------------------------------------------------------------------- |
| feePerTx   | string | Estimated total fee in litoshis for a transaction of the supplied `txsize` |
| feePerUnit | string | Estimated fee rate in litoshis per **kilobyte**, not per vbyte             |

{% hint style="warning" %}
`feePerUnit` is quoted per 1000 bytes, following the node's `estimatesmartfee`, which reports LTC per kilobyte. It is an integer count of litoshis, not a decimal LTC amount. Dividing by 1000 gives the litoshis/vB rate wallets display: the `994` above is roughly **0.99 litoshis/vB**, not 994. Treating it as a per-byte rate overpays by three orders of magnitude.

`feePerTx` is derived as `txsize x feePerUnit / 1000`; in the response above, `225 x 994 / 1000 = 224`.

Litecoin's relay minimum keeps the estimate flat across targets when the mempool is uncongested, so identical values for 1, 6 and 12 blocks are expected rather than a fault.
{% endhint %}

## Use Cases

* **Fee Selection**: Choose a rate matching the confirmation speed a payment needs
* **Cost Preview**: Show the user the fee before signing
* **Tiered Options**: Offer fast, standard, and economy targets from one call
* **Batch Planning**: Size a consolidation against the current rate

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| error                     | Invalid params | The blocks array is missing or malformed          |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
