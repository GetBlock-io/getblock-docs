---
description: >-
  Example code for the estimateFee WebSocket method. Complete guide on how to use
  the estimateFee WebSocket method in the GetBlock Web3 documentation.
---

# estimateFee - Dogecoin

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
            "feePerTx": "-22500000",
            "feePerUnit": "-100000000"
        },
        {
            "feePerTx": "225574",
            "feePerUnit": "1002550"
        },
        {
            "feePerTx": "225561",
            "feePerUnit": "1002495"
        }
    ]
}
```

Results are returned in the same order as the requested `blocks` targets.

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| feePerTx | string | Estimated total fee in koinu for a transaction of the supplied `txsize` |
| feePerUnit | string | Estimated fee rate in koinu per **kilobyte**, not per byte |

{% hint style="danger" %}
**The one-block target returns negative values.** Look at the first entry above: `feePerUnit: "-100000000"` and `feePerTx: "-22500000"`.

The node's `estimatesmartfee` reports `-1` when it has no estimate for a target, and Blockbook scales that into koinu per kilobyte: `-1 DOGE/kB` is `-100000000` koinu/kB. `feePerTx` is then derived from it, giving `225 x -100000000 / 1000 = -22500000`.

A client that takes the first entry and multiplies it out produces a **negative fee** and builds an invalid transaction. Always check that `feePerUnit` is positive before using an entry, and fall back to a fixed minimum or a longer target when it is not. The REST [api/v2/estimatefee](../dogecoin-blockbook-rest-api/api-v2-estimatefee-dogecoin.md) endpoint surfaces the same condition as the string `-1`.
{% endhint %}

{% hint style="warning" %}
`feePerUnit` is quoted per 1000 bytes, as an integer count of koinu rather than a decimal DOGE amount. Dividing by 1000 gives koinu per byte: the `1002550` above is roughly **1002 koinu/byte**, or about 0.01 DOGE per kilobyte.

`feePerTx` is derived as `txsize x feePerUnit / 1000`; in the second entry, `225 x 1002550 / 1000 = 225574`.

The curve is steep at the fast end. A two-block target costs roughly fifty times a six-block target, so treating a faster target as a small premium will overpay badly on Dogecoin.
{% endhint %}

## Use Cases

* **Fee Selection**: Choose a rate matching the confirmation speed a payment needs
* **Cost Preview**: Show the user the fee before signing
* **Tiered Options**: Offer standard and economy targets from one call
* **Batch Planning**: Size a consolidation against the current rate

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| error | Invalid params | The blocks array is missing or malformed |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
