---
description: >-
  This contain all the endpoints, pricing and error codes to access Address
  Audit service
---

# API Reference

The GetBlock Address Audit API provides access to all address audit services, which you can integrate into your dApp, including wallet audit, wallet riska nd rug pull checker

### Base URL

{% code overflow="wrap" %}
```bash
https://services.getblock.io/v1
```
{% endcode %}

### Authentication

| Services         | URL                                                  |
| ---------------- | ---------------------------------------------------- |
| Wallet Audit     | `https://services.getblock.io/v1/wallet-audit/audit` |
| Wallet Risk      | `https://services.getblock.io/v1/wallet-audit/check` |
| Rug Pull Checker | `https://services.getblock.io/v1/rug-pull/check`     |

All requests require an API key in the header:

```bash
Authorization: Bearer <API_KEY>
```

### How to Get Address Audit API Key

1. Go to your [account dashboard](https://account.getblock.io/)
2. Click on the product icon on the Navbar

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

3. Scroll down and Select **Address Audit**
4. After that, select the **API key** tab
5. On the **API Key** tab, click on the plus icon, and your API key will be generated for you automatically

<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Ensure you store your API Key securely
{% endhint %}

### QuickStart

In this example, you will be auditing a wallet address:&#x20;

* **Axios** (JavaScript / Node.js)
* **Python** (Requests library)

Before you begin, you must have already installed [`npm`](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [`yarn`](https://classic.yarnpkg.com/lang/en/docs/install) on your local machine (for the Axios example) or Python and pip (for the Python example).

{% tabs %}
{% tab title="JavaScript (Axios)" %}
{% stepper %}
{% step %}
#### Setup project

Create and initialize a new project:

```bash
mkdir wallet-audit-quickstart
cd wallet-audit-quickstart
npm init --yes
```
{% endstep %}

{% step %}
#### Install Axios

```bash
npm install axios
```
{% endstep %}

{% step %}
#### Create file

Create a new file named `index.js`. This is where you will make your first call.
{% endstep %}

{% step %}
#### Set ES module type

Set the ES module `"type": "module"` in your `package.json`.
{% endstep %}

{% step %}
#### Add code

Add the following code to `index.js`:

{% code title="index.js" %}
```javascript
import axios from "axios";

const data = 
  {  "network": "ETH", "address": "0xa45f...a675"};

const config = {
  method: "post",
  url: "https://services.getblock.io/v1/wallet-audit/check",
  headers: {
    Authorization: "Bearer YOUR_API_KEY",
    "Content-Type": "application/json",
  },
  data: data,
};

axios(config)
  .then((response) => console.log(JSON.stringify(response.data, null, 2)))
  .catch((error) => console.log(error));

```
{% endcode %}

> Replace `<YOUR_API_KEY>` with your actual API Key from the GetBlock dashboard.
{% endstep %}

{% step %}
#### Run the script

```bash
node index.js
```

Expected output (example):

```json
{
    "data": {
        "message": "Success",
        "walletAddress": "0xa45f...a675",
        "status": "Fraud",
        "probabilityFraud": "0.9329851866",
        "token": null,
        "chain": "ETH",
        "lastChecked": "2026-03-19T07:02:56.000Z",
        "forensic_details": {
            "cybercrime": "0",
            "money_laundering": "0",
            "number_of_malicious_contracts_created": "0",
            "gas_abuse": "0",
            "financial_crime": "0",
            "darkweb_transactions": "0",
            "reinit": "0",
            "phishing_activities": "0",
            "fake_kyc": "0",
            "blacklist_doubt": "0",
            "fake_standard_interface": "0",
            "data_source": "",
            "stealing_attack": "0",
            "blackmail_activities": "0",
            "sanctioned": "0",
            "malicious_mining_activities": "0",
            "mixer": "0",
            "fake_token": "0",
            "honeypot_related_address": "0"
        },
        "checked_times": 3,
        "createdAt": "2026-03-19T06:52:28.000Z",
        "updatedAt": "2026-05-15T17:01:48.000Z",
        "sanctionData": [
            {
                "id": 205813,
                "trustscore_id": 18360032,
                "category": null,
                "name": null,
                "description": null,
                "url": null,
                "isSanctioned": false,
                "createdAt": "2026-05-15T17:01:48.000Z",
                "updatedAt": "2026-05-15T17:01:48.000Z"
            }
        ]
    }
}
```
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% stepper %}
{% step %}
#### Setup the project directory

```bash
mkdir wallet-audit-quickstart
cd wallet-audit-quickstart
```
{% endstep %}

{% step %}
#### Create and activate a virtual environment

```bash
python -m venv venv
source venv/bin/activate

# On Windows, use:
venv\Scripts\activate
```
{% endstep %}

{% step %}
#### Install requests

```bash
pip install requests
```
{% endstep %}

{% step %}
#### Create script

Create a file called `main.py` with the following content:

{% code title="main.py" overflow="wrap" %}
```python
import requests
import json

url = "https://services.getblock.io/v1/wallet-audit/check"

payload = json.dumps({  "network": "ETH", "address": "0xa45f...a675"})

headers = {
      "Authorization": "Bearer YOUR_API_KEY",
  "Content-Type": "application/json",
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}

> Replace `<YOUR_API_KEY>` with your actual GetBlock API Key.
{% endstep %}

{% step %}
#### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

### Next Step

* [Wallet Risk endpoint](wallet-risk-endpoint.md)
* [Wallet Audit endpoint](wallet-audit-endpoint.md)
* Rug Pull Checker endpoint
