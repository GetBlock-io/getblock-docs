---
description: >-
  Example code for the estimateFee WebSocket method. Complete guide on how to
  use the estimateFee WebSocket method in the GetBlock Web3 documentation.
---

# estimateFee - Dash

Returns fee-rate estimates for one or more confirmation-block targets over WebSocket.

## Parameters

| Parameter | Type  | Required | Description                                     |
| --------- | ----- | -------- | ----------------------------------------------- |
| blocks    | array | Yes      | Array of confirmation targets, e.g. \[1, 6, 12] |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/websocket

# then send:
{
    "id": "getblock.io",
    "method": "estimateFee",
    "params": {
        "blocks": [
            1,
            6,
            12
        ],
        "specific": {
            "conservative": true,
            "txsize": 1278
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
            "feePerTx": "1278",
            "feePerUnit": "1000"
        },
        {
            "feePerTx": "1278",
            "feePerUnit": "1000"
        },
        {
            "feePerTx": "1278",
            "feePerUnit": "1000"
        }
    ]
}
```

## Response Fields

| Field      | Type   | Description                                                            |
| ---------- | ------ | ------------------------------------------------------------------------ |
| feePerTx   | string | Estimated total fee in duffs for a transaction of the supplied `txsize` |
| feePerUnit | string | Estimated fee rate in duffs per **kilobyte**, not per byte              |

Results are returned in the same order as the requested `blocks` targets.

{% hint style="warning" %}
`feePerUnit` is quoted per 1000 bytes, following the backend's `estimatesmartfee`, which reports DASH per kilobyte. It is an integer count of duffs, not a decimal DASH amount. `feePerTx` is derived as `txsize x feePerUnit / 1000`; in the response above, `1278 x 1000 / 1000 = 1278` duffs.

Dash's relay minimum keeps the estimate flat at 1000 duffs/kB across all three targets when the mempool is uncongested, so identical values for 1, 6 and 12 blocks are expected rather than a bug.
{% endhint %}

## Use Cases

* **Fee Selection**: Fetch several targets at once
* **Wallets**: Offer fee tiers
* **Batching**: Pick economical fees

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| error                     | Bad request   | The targets are invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
