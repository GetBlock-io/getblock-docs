---
description: Example code for the delegateEnergy JSON RPC method
---

# delegateEnergy

This endpoint delegates energy to a TRON address. In most cases, the order completes immediately with status "success".

#### Parameter

| Parameter       | Type    | Required | Description                                        |
| --------------- | ------- | -------- | -------------------------------------------------- |
| target\_address | string  | Yes      | TRON wallet address (starts with T, 34 characters) |
| volume          | integer | Yes      | Energy amount: 30,000 — 5,000,000                  |
| duration        | string  | Yes      | "1h", "1d", "3d", "7d"                             |

### Idempotency

delegateEnergy charges your balance and may place an on-chain order. To make retries safe, send a unique key with each _intent_:

```
Idempotency-Key: 6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44
```

A UUID is recommended. Generate a **new** key for every new purchase, and reuse the **same** key when retrying that exact purchase.

| Situation                                         | Result                                                                |
| ------------------------------------------------- | --------------------------------------------------------------------- |
| Same key + **same** body                          | The original response is replayed. No second charge, no second order. |
| Same key + **different** body                     | `422`                                                                 |
| Same key on a **different** endpoint              | `409`                                                                 |
| Duplicate sent while the first is still in flight | `409` with a `Retry-After` header                                     |
| Idempotency store temporarily unavailable         | `503` with a `Retry-After` header (retry shortly)                     |

{% hint style="info" %}
Always send an idempotency key on write endpoints. It is the difference between a safe retry and an accidental double on-chain charge.
{% endhint %}

#### Request Sample

```json
curl -X POST https://api.getblock.io/tron-energy/delegateEnergy \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: 6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44" \
  -d '{
    "target_address": "TUo8pycbvje...zw67bpPs4GLFyD",
    "volume": 65000,
    "duration": "1h"
  }'
```

#### &#x20;Response&#x20;

```bash
{
  "data": {
    "status": "success",
    "target_address": "TUo8...",
    "resource_type": "energy",
    "volume": 65000,
    "duration": "1h",
    "price_usd": "0.59",
    "trx_spent": 3,
    "order_id": "1H76d06176b8",
    "txid": "db47c131876a504ceee32f023a4aa3aa23629362e...",
    "message": "Delegation completed successfully."
  }
}
```
