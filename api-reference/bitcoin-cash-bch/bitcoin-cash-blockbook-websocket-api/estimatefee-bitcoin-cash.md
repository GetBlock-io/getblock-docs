---
description: >-
  Example code for the estimateFee WebSocket method. Complete guide on how to use
  the estimateFee WebSocket method in the GetBlock Web3 documentation.
---

# estimateFee - Bitcoin Cash

Returns the backend fee estimate for one or more confirmation targets. Supplying the transaction's size returns the estimated total fee alongside the per-unit rate.

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| blocks | array | Yes | Confirmation targets in blocks, one estimate returned per entry |
| specific | object | No | Chain-specific options: `conservative` for smart fee mode, `txsize` in bytes |

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
            "txsize": 226
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
            "feePerTx": "226",
            "feePerUnit": "1000"
        },
        {
            "feePerTx": "226",
            "feePerUnit": "1000"
        },
        {
            "feePerTx": "226",
            "feePerUnit": "1000"
        }
    ]
}
```

Results are returned in the same order as the requested `blocks` targets.

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| feePerTx | string | Estimated total fee in satoshis for a transaction of the supplied `txsize` |
| feePerUnit | string | Estimated fee rate in satoshis per **kilobyte**, not per byte |

{% hint style="warning" %}
`feePerUnit` is quoted per 1000 bytes, following the node's `estimatesmartfee`, which reports BCH per kilobyte. It is an integer count of satoshis, not a decimal BCH amount. Dividing by 1000 gives the sat/byte rate wallets display: the `1000` above is **1 sat/byte**, the network relay minimum.

`feePerTx` is derived as `txsize x feePerUnit / 1000`; in the response above, `226 x 1000 / 1000 = 226`.

Bitcoin Cash blocks are rarely full, so the estimate usually sits at the 1 sat/byte relay minimum for every target. Identical values across 1, 6 and 12 blocks are expected rather than a fault.
{% endhint %}

## Use Cases

* **Fee Selection**: Choose a rate matching the confirmation speed a payment needs
* **Cost Preview**: Show the user the fee before signing
* **Batch Planning**: Size a consolidation against the current rate

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| error | Invalid params | The blocks array is missing or malformed |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
