---
description: This contain all the endpoints, pricing and error codes to access TRON energy
---

# API Reference

The GetBlock TRON Energy API lets you programmatically delegate Energy and Bandwidth to any TRON address. Automate fee optimization for exchanges, payment services, and dApps.

### Base URL

```bash
https://services.getblock.io/v1/tron-energy
```

### Authentication

All requests require an API key in the header:

```bash
Authorization: Bearer YOUR_API_KEY
```

{% hint style="info" %}
**Getting your API key**\
\
Tron Energy accepts any GetBlock API key. Generate one under **Settings → API Keys** in your GetBlock dashboard (the Tron Energy **API** tab links there). **Copy and save it right away** — it is shown only once. Treat it as a secret: anyone holding your API key can spend the credits you've topped up on the platform. Store it following security best practices (never commit it to code or share it in plain text).
{% endhint %}

### Request example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://services.getblock.io/v1/tron-energy/delegate-energy \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: 6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44" \
  -d '{"target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD", "volume": 65000, "duration": "1h"}'
```
{% endtab %}

{% tab title="Response" %}
All responses return JSON wrapped in a `data` object:

```json
{
  "data": {
    "orderId": "0a6d59c4-9486-45b1-bf3f-74d075ad9b4c",
    "status": "success",
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "resource_type": "energy",
    "volume": 65000,
    "duration": "1h",
    "price_usd": "0.40",
    "trx_spent": 2,
    "txid": "63285f35942a0a8552fa7664b5fd8d4c2140640e9c766fe44fda7e01c97233df"
  }
}
```
{% endtab %}
{% endtabs %}

### Endpoints

| Endpoint              | Method | Description                                                   |
| --------------------- | ------ | ------------------------------------------------------------- |
| `/price-estimate`     | POST   | Get a real-time price quote for Energy or Bandwidth           |
| `/delegate-energy`    | POST   | Delegate 32,000–5,000,000 Energy for 15 minutes to 14 days    |
| `/delegate-bandwidth` | POST   | Delegate 1,000–50,000 Bandwidth                               |
| `/address-activate`   | POST   | Activate a new (never-used) TRON address on-chain             |
| `/orders/{order_id}`  | GET    | Retrieve the status of a previously placed order              |

### Limits

| Parameter          | Energy                                    | Bandwidth             |
| ------------------ | ----------------------------------------- | --------------------- |
| `volume`           | 32,000 – 5,000,000                        | 1,000 – 50,000        |
| `duration`         | `15m`, `1h`, `1d`, `3d`, `7d`, `14d`      | `1h`, `3d`, `7d`, `14d` |

### Errors

Errors return the same envelope with `data: null` and a human-readable `error` message:

```json
{
  "data": null,
  "error": "energy volume must be between 32000 and 5000000"
}
```

| HTTP code | Meaning                                                                                  |
| --------- | ---------------------------------------------------------------------------------------- |
| `200`     | Request completed                                                                        |
| `202`     | Order accepted, awaiting on-chain confirmation — poll `/orders/{order_id}`               |
| `400`     | Invalid body: wrong `duration`, `volume` out of range, invalid `target_address`, etc.    |
| `401`     | `Missing or invalid Authorization header`                                                |
| `404`     | `order not found`                                                                        |
| `409`     | Idempotency conflict — same key on a different endpoint, or a duplicate still in flight  |
| `422`     | Same `Idempotency-Key` reused with a different body                                      |

### Pricing

Orders are paid from your GetBlock **Credits** balance and priced in USD. Prices are live and change over time, so get a fresh quote from [`price-estimate`](price-estimate-tron-energy.md) before each order.

Your balance is charged when an order reaches status `charged`; the order's `charged_at` and `price_usd` show when and how much. The final charge can differ from the `price-estimate` quote — for example, a $0.23 quote charged $0.30 — so leave some margin on your balance.

{% hint style="warning" %}
**Longer rentals cost much more.** The price is per unit of Energy for the whole rental period, so it grows with duration. For a single USDT transfer (\~65,000 Energy), a 1-hour rental cost 1.2–1.9 TRX at the time of writing (November 2026), while a 1-day rental cost about 8.6 TRX — more than the \~6.5 TRX you would burn without renting. Pick the shortest duration that covers your transactions and always check [`price-estimate`](price-estimate-tron-energy.md) first.
{% endhint %}

