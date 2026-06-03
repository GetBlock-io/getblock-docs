---
description: This contain all the endpoints, pricing and error codes to access TRON energy
---

# API Reference

The GetBlock TRON Energy API lets you programmatically delegate Energy and Bandwidth to any TRON address. Automate fee optimization for exchanges, payment services, and dApps.

### Base URL

```bash
https://services.getblock.io/v1/tron-energy/
```

### Authentication

All requests require an API key in the header:

```bash
Authorization: Bearer YOUR_API_KEY
```

{% hint style="info" %}
**Getting your API key**\
\
Generate API keys under **Settings → API**. Open **Settings**, go to the **API** tab, and create a key for your needs. **Copy and save it right away** — treat it as a secret: anyone holding your API key can spend the credits you've topped up on the platform. Store it following security best practices (never commit it to code or share it in plain text).
{% endhint %}

### Request example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://services.getblock.io/v1/tron-energy/delegate-energy \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: 6f9c2a1e-3b7d-4c08-9f21-2e5a7c0b1d44" \
  -d '{"target_address": "TUo8...", "volume": 65000, "duration": "1d"}'

```
{% endtab %}

{% tab title="Response" %}
All responses return JSON. Successful delegations include:

```json
{
  "data": {
    "status": "success",
    "target_address": "TUo8...",
    "resource_type": "energy",
    "volume": 65000,
    "duration": "1d",
    "price_usd": "0.79",
    "trx_spent": 3,
    "order_id": "1H76d06176b8",
    "txid": "db47c131876a504ceee32f023a46cf3aa23629362e28c8c272c10ced8f1f27a8",
    "message": "Delegation completed successfully."
  }
}

```
{% endtab %}
{% endtabs %}

### Endpoints

| Endpoint          | Method | Description                           |
| ----------------- | ------ | ------------------------------------- |
| `/price-estimate` | POST   | Get real-time price quote             |
| `/delegateEnergy` | POST   | Delegate Energy (30K–5M) for 5 min–7d |

### Pricing

You pay in **Credits**. The price is calculated from two live inputs:

* the current TRX/USD exchange rate, and
* real-time energy demand on the network.

You are charged only after the delegation is confirmed — if the order fails, nothing is deducted from your balance.

**Energy prices fluctuate over time.** Demand follows on-chain activity across the world's major financial clusters — **Asia, Europe, and the US** — so the price rises and falls as different time zones become active and quiet through the day. The swing can be large: in calmer windows energy can be significantly cheaper — sometimes around half the price of peak hours.

{% hint style="info" %}
If getting a lower price matters to you, **monitor the live price over time and rent when it fits your economic needs.** There is no fixed "cheap hour" — because activity shifts between regions, the best window varies, so watch the actual numbers rather than relying on a fixed schedule.
{% endhint %}
