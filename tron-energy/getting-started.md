---
description: >-
  Learn how to access Tron energy, top your balance, delegate resources, check
  prices and manage API key from your dashboard.
---

# Getting Started

The Dashboard tab gives you a comprehensive overview of your activity:

* USD Balance: your current balance with a quick Top Up button
* Usage: bar chart showing delegation activity (filter by 24h, 7 days, month, or all time)
* Deposit History: table of all top-ups with date, method, and amount
* TRON Energy Activity: full order history with address, type, volume, duration, price, status, and date
* Export: download order history as XLS or PDF

In this guide, you will learn how to easily navigate through the dashboard to get the following:

1. How to gain access to Tron energy
2. How to top up your balanace&#x20;
3. Delegate Resources
4. Check Prices
5. Manage API Keys

### How to Access Dashboard

{% stepper %}
{% step %}
Create an account on [GetBlock](https://account.getblock.io) using Gmail, Metamask, or GitHub
{% endstep %}

{% step %}
Under **products**, click on **Tron energy**. The system will automatically sync your GetBlock account with the Energy service.

The dashboard consists of three main tabs:

1. Dashboard — view your balance, usage charts, deposit history, and order activity
2. Delegate — delegate Energy or Bandwidth to TRON addresses, manage API keys
3. API Docs — reference documentation with cURL examples for all endpoints
{% endstep %}
{% endstepper %}

### How to Top Up Your Balance

Before you can delegate resources, you need to fund your USD balance:

{% stepper %}
{% step %}
Click the "Top Up" button in the balance bar at the top of the dashboard
{% endstep %}

{% step %}
Select a preset amount ($50, $100, $500) or enter a custom amount (minimum $10)
{% endstep %}

{% step %}
Choose your payment method: Card or Crypto
{% endstep %}

{% step %}
Complete the payment — your balance updates instantly

{% hint style="info" %}
Your USD balance is shared across all GetBlock products.
{% endhint %}
{% endstep %}
{% endstepper %}

### How to Delegate Resources

Go to the Delegate tab and select Manual mode.

#### 1. Delegate Energy

{% stepper %}
{% step %}
Select the Energy sub-tab
{% endstep %}

{% step %}
Enter the target TRON address (starts with T, 34 characters)
{% endstep %}

{% step %}
Set the Energy amount (30,000–5,000,000)
{% endstep %}

{% step %}
Choose the duration: 1h, 3h, 6h, 12h, or 1 day
{% endstep %}

{% step %}
Click "Delegate Energy"
{% endstep %}
{% endstepper %}

#### 2. Delegate Bandwidth

{% stepper %}
{% step %}
Select the Bandwidth sub-tab
{% endstep %}

{% step %}
Enter the target TRON address
{% endstep %}

{% step %}
Set the Bandwidth amount (1,000–200,000)
{% endstep %}

{% step %}
Duration is fixed at 1 hour for Bandwidth
{% endstep %}

{% step %}
Click "Delegate Bandwidth"
{% endstep %}
{% endstepper %}

#### 3. Activate Address

If you need to delegate to a brand-new TRON address that has never been used, you must activate it first:

{% stepper %}
{% step %}
Select the Activate sub-tab
{% endstep %}

{% step %}
Enter the target TRON address
{% endstep %}

{% step %}
Click "Activate Address=-098

{% hint style="info" %}
Activation costs a fixed 1.87 TRX, charged in USD at the current TRX/USD rate.
{% endhint %}
{% endstep %}
{% endstepper %}

### How to Check Price

The Check Price panel is available on the right side of the Delegate form (Manual mode). Use it to get a real-time estimate before placing an order:

{% stepper %}
{% step %}
Enter Energy amount (30,000–5,000,000)
{% endstep %}

{% step %}
Select duration: 1 hour, 1 day, 3 days, 7 days, or 14 days
{% endstep %}

{% step %}
Click "Check Price"
{% endstep %}

{% step %}
The result shows: price in SUN per unit, total TRX, and total USD
{% endstep %}
{% endstepper %}

### How to Manage API Keys

To use the API, you need an API key:

{% stepper %}
{% step %}
Go to Delegate → API mode
{% endstep %}

{% step %}
Click "+ Add Key" (requires a funded balance)
{% endstep %}

{% step %}
Copy and securely store the key immediately — it is shown only once
{% endstep %}

{% step %}
You can create up to 5 API keys
{% endstep %}

{% step %}
To revoke a key, click "Revoke" next to it — the key stops working immediately
{% endstep %}
{% endstepper %}

### Next Steps

1. [Tron Fee Mode](tron-fee-model.md)
2. [API Reference](api-reference/)
