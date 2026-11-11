---
description: >-
  Learn how to open Tron Energy, rent Energy and Bandwidth, activate addresses,
  review your order history, and get an API key from your GetBlock dashboard.
---

# Getting Started

In this guide, you will learn how to use Tron Energy from your GetBlock dashboard:

1. Open Tron Energy
2. Fund your Credits balance
3. Rent Energy
4. Rent Bandwidth
5. Activate an address
6. Review your history
7. Get an API key

### How to Open Tron Energy

{% stepper %}
{% step %}
Sign in to your [GetBlock account](https://account.getblock.io) using Gmail, MetaMask, or GitHub
{% endstep %}

{% step %}
In the sidebar, open **Payments** and click **Tron Energy**.

The page has three tabs:

1. **Rent** — rent Energy or Bandwidth, or activate an address
2. **History** — usage and a list of all your orders
3. **API** — how to call Tron Energy from your own code

Your **Credits** balance is shown in the top-right corner of the page.
{% endstep %}
{% endstepper %}

### How to Fund Your Balance

Rentals and activations are paid from your **Credits** balance — prepaid service credits used to pay for premium features.

{% stepper %}
{% step %}
Click the **Credits** balance in the top-right corner of the Tron Energy page
{% endstep %}

{% step %}
In the panel that opens, click **Top up** and complete the payment
{% endstep %}
{% endstepper %}

{% hint style="info" %}
Your Credits balance is shared across all GetBlock products.
{% endhint %}

### How to Rent Energy

<figure><img src="../.gitbook/assets/tron-energy-rent-energy.png" alt="Rent Energy form with TRON wallet address, Energy, Duration, and estimated cost"><figcaption><p>Rent → Energy</p></figcaption></figure>

{% stepper %}
{% step %}
Go to **Rent** and select the **Energy** sub-tab
{% endstep %}

{% step %}
Enter the target **TRON wallet address** (starts with T, 34 characters)
{% endstep %}

{% step %}
Enter the **Energy** amount. A typical USDT transfer needs about 65,000 Energy, or about 131,000 if the recipient has never held USDT.
{% endstep %}

{% step %}
Choose the **Duration**: 1 hour, 1 day, or 3 days
{% endstep %}

{% step %}
Check the **Estimated cost** under the form, then click **Rent Energy**
{% endstep %}
{% endstepper %}

{% hint style="warning" %}
The price grows with duration. For a single USDT transfer, 1 hour is usually enough — a 1-day rental can cost more than paying the network fee in TRX. See [Pricing](api-reference/#pricing).
{% endhint %}

### How to Rent Bandwidth

<figure><img src="../.gitbook/assets/tron-energy-rent-bandwidth.png" alt="Rent Bandwidth form with TRON wallet address, Bandwidth Amount, Duration, and estimated cost"><figcaption><p>Rent → Bandwidth</p></figcaption></figure>

{% stepper %}
{% step %}
Go to **Rent** and select the **Bandwidth** sub-tab
{% endstep %}

{% step %}
Enter the target **TRON wallet address**
{% endstep %}

{% step %}
Enter the **Bandwidth Amount** (the form shows the allowed minimum and maximum)
{% endstep %}

{% step %}
Leave **Duration** at **1 hour** — the only option in the dashboard
{% endstep %}

{% step %}
Check the **Estimated cost**, then click **Rent Bandwidth**
{% endstep %}
{% endstepper %}

### How to Activate an Address

A brand-new TRON address that has never received TRX must be activated before it can receive Energy or Bandwidth.

<figure><img src="../.gitbook/assets/tron-energy-activate.png" alt="Activate Address form with TRON wallet address and estimated cost"><figcaption><p>Rent → Activate</p></figcaption></figure>

{% stepper %}
{% step %}
Go to **Rent** and select the **Activate** sub-tab
{% endstep %}

{% step %}
Enter the target **TRON wallet address**
{% endstep %}

{% step %}
Check the **Estimated cost**, then click **Activate Address**
{% endstep %}
{% endstepper %}

### How to Review Your History

Open the **History** tab to see:

* **Usage** — the number of successful rentals, filtered by 24 hours, 7 days, Month, or All time
* **History** — every order with its date, type (Energy rental, Bandwidth rental, or Wallet activation), target address, volume, price in SUN, and amount charged

### How to Get an API Key

Tron Energy endpoints accept any GetBlock API key. Keys are managed in your account settings and work across every GetBlock service that supports API key authentication.

<figure><img src="../.gitbook/assets/tron-energy-api-tab.png" alt="API tab with steps to create a key, authenticate requests, and rotate or revoke keys"><figcaption><p>API tab</p></figcaption></figure>

{% stepper %}
{% step %}
Open the **API** tab and click **Manage API keys in Settings**, or go to **Settings → API Keys**
{% endstep %}

{% step %}
Generate a new key. The full key is shown only once — copy and store it securely right away.
{% endstep %}

{% step %}
Send the key as a Bearer token in the `Authorization` header of every Tron Energy request:

```bash
Authorization: Bearer YOUR_API_KEY
```
{% endstep %}

{% step %}
To revoke a key you no longer use, return to **Settings → API Keys**. Revocation takes effect immediately.
{% endstep %}
{% endstepper %}

### Next Steps

1. [TRON Fee Model](tron-fee-model.md)
2. [API Reference](api-reference/)
3. [How to Cut USDT Transfer Fees on TRON by Renting Energy with GetBlock](../../guides/how-to-cut-usdt-transfer-fees-on-tron-by-renting-energy-with-getblock.md)
