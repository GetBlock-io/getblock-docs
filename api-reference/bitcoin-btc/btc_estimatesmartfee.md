---
description: >-
  Example code for the estimatesmartfee JSON RPC method. Сomplete guide on how
  to use estimatesmartfee JSON RPC in GetBlock Web3 documentation.
---

# estimatesmartfee - Bitcoin

This method estimates the approximate fee per kilobyte needed for a transaction to begin confirmation within a given number of blocks.

### Parameters

| Parameter      | Type   | Required | Description                                                                                |
| -------------- | ------ | -------- | ------------------------------------------------------------------------------------------ |
| conf\_target   | number | Yes      | Confirmation target in blocks (1 - 1008).                                                  |
| estimate\_mode | string | No       | The fee estimate mode: "unset", "economical", or "conservative" (default: "conservative"). |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "estimatesmartfee",
    "params": [6, "economical"],
    "id": "getblock.io"
}'
```
{% endtab %}
{% endtabs %}

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "feerate": 0.00012345,
        "blocks": 6
    }
}
```

### Response Parameters

| Field   | Type   | Description                                      |
| ------- | ------ | ------------------------------------------------ |
| feerate | number | Estimate fee rate in BTC/kB (1 kB = 1000 bytes). |
| blocks  | number | Block number where estimate was found.           |
| errors  | array  | Errors encountered during processing (if any).   |

### Use Case

The `estimatesmartfee` method is essential for:

* Dynamic fee calculation for transactions
* Building user fee selection interfaces
* Implementing RBF (Replace-By-Fee) strategies
* Optimizing transaction costs
* Supporting various confirmation time preferences
* Building fee estimation dashboards

### Error Handling

| Status Code | Error Message     | Cause                                |
| ----------- | ----------------- | ------------------------------------ |
| 403         | Forbidden         | Missing or invalid ACCESS-TOKEN.     |
| -1          | Insufficient data | Not enough data to make an estimate. |

### Integration Notes

The `estimatesmartfee` method helps developers:

* Build dynamic fee calculators
* Implement priority-based fee selection
* Create cost-optimized transaction systems
* Support time-sensitive transactions
* Build fee comparison tools
