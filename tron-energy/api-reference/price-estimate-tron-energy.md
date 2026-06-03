---
description: Example code for the price-estimate JSON RPC method
---

# price-estimate

This get a real-time price estimate for Energy delegation before placing an order.

### Body Parameter

| Parameter    | Type    | Required | Description                                             |
| ------------ | ------- | -------- | ------------------------------------------------------- |
| resourceType | string  | Yes      | "energy", "bandwidth"(soon)                             |
| volume       | integer | Yes      | Energy amount: 30,000 - 5,000,000 (depends on duration) |
| duration     | string  | Yes      | "1h", "1d", "3d", "7d"                                  |

### Request Sample

```bash
curl -X POST https://services.getblock.io/v1/tron-energy/price-estimate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"resourceType": "energy", "volume": 65000, "duration": "1d"}'
```

### Response Sample

```bash
{
  "data": {
    "price_sun": "36",
    "trx": "2.3400",
    "price_usd": "0.7900",
    "reserve_usd": "0.9500"
  }
}
```
