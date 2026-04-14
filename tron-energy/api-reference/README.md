---
description: This contain all the endpoints, pricing and error codes to access TRON energy
---

# API Reference

The GetBlock TRON Energy API lets you programmatically delegate Energy and Bandwidth to any TRON address. Automate fee optimization for exchanges, payment services, and dApps.

### Base URL

```bash
https://api.getblock.io/tron-energy
```

### Authentication

All requests require an API key in the header:

```bash
Authorization: Bearer YOUR_API_KEY
```

### Quick Example

* Generate keys in your dashboard under **Delegate → API**.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://api.getblock.io/tron-energy/delegateEnergy \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"target_address": "TUo8...", "volume": 65000, "duration": "1d"}'
```
{% endtab %}

{% tab title="Response" %}
All responses return JSON. Successful delegations include:

```json
{
  "order_id": "clxyz123...",
  "status": "success",
  "price_usd": "1.58",
  "txid": "abc123..."
}
```
{% endtab %}
{% endtabs %}

### Endpoints

| Endpoint              | Method | Description                         |
| --------------------- | ------ | ----------------------------------- |
| `/delegateEnergy`     | POST   | Delegate Energy (30K–5M) for 1h–14d |
| `/delegateBandwidth`  | POST   | Delegate Bandwidth (1K–200K) for 1h |
| `/addressActivate`    | POST   | Activate a new TRON address         |
| `/orders/{order_id}`  | GET    | Check order status                  |
| `/api/price-estimate` | POST   | Get real-time price quote           |

### Rate Limits

* **30 requests/minute** per API key
* Up to **5 API keys** per account

### Error Codes

<table data-header-hidden><thead><tr><th width="138.765625"></th><th></th></tr></thead><tbody><tr><td>HTTP Status</td><td>Meaning</td></tr><tr><td>400</td><td>Invalid request parameters (bad address, volume, duration, or resource type)</td></tr><tr><td>401</td><td>Missing or invalid API key</td></tr><tr><td>402</td><td>Insufficient balance — top up your account</td></tr><tr><td>429</td><td>Rate limit exceeded (30 requests/min per API key)</td></tr><tr><td>502</td><td>Provider error — the upstream delegation service is temporarily unavailable</td></tr></tbody></table>

### Pricing

You pay in USD. The price is calculated based on the current TRX/USD rate. You are charged only after the delegation is confirmed — if the order fails, nothing is deducted.

For example, 32,000 Energy for 24h costs approximately $1.60–$1.80, depending on the current TRX rate.
