---
description: >-
  GetBlock provides fast and reliable access to Stellar nodes via JSON-RPC API.
  Connect to the Stellar network without running your own infrastructure.
---

# Stellar (XLM)

Stellar is a decentralized Layer-1 blockchain designed for fast, low-cost cross-border payments and asset tokenization. Built on the Stellar Consensus Protocol (SCP), it achieves consensus through federated Byzantine agreement rather than mining, enabling near-instant transaction finality with minimal energy consumption. Stellar supports the Soroban smart contract platform for building decentralized applications.

### Key Features

* **Stellar Consensus Protocol (SCP)**: Proof of Agreement consensus for fast, energy-efficient finality
* **Native Currency XLM**: Lumens power transaction fees and serve as a bridge currency
* **\~5 Second Ledger Close**: Near-instant transaction confirmation
* **Minimal Transaction Fees**: Fractions of a cent per transaction
* **Soroban Smart Contracts**: WebAssembly-based smart contract platform
* **Built-in DEX**: Native decentralized exchange for asset trading
* **Asset Tokenization**: Issue and manage custom assets on-chain
* **Cross-Border Payments**: Designed for global financial infrastructure

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and streamlined developer experience optimization. The canonical and normative specification for Stellar RPC methods is solely maintained and published through the official Stellar Development Foundation documentation portal at_ [_developers.stellar.org_](https://developers.stellar.org/)_. This resource constitutes the sole authoritative reference implementation of the JSON-RPC 2.0 protocol interface for Stellar nodes._
{% endhint %}

## Network Information

| Property          | Value                            |
| ----------------- | -------------------------------- |
| Network Name      | Stellar                          |
| Native Currency   | XLM (Lumens)                     |
| Decimals          | 7 (stroops)                      |
| Consensus         | Stellar Consensus Protocol (SCP) |
| Ledger Close Time | \~5 seconds                      |
| Base Reserve      | 0.5 XLM                          |
| Minimum Balance   | 1 XLM (2 base reserves)          |
| Smart Contracts   | Soroban                          |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://go.getblock.io
```
{% endtab %}

{% tab title="Singapore, Singapore" %}
```
https://go.getblock.asia
```
{% endtab %}

{% tab title="New York, USA" %}
```
https://go.getblock.us
```
{% endtab %}
{% endtabs %}

## Supported Networks

<table><thead><tr><th>Network</th><th>JSON RPC</th><th>REST</th><th>New York, USA</th><th>Frankfurt, Germany</th><th>Singapore, Singapore</th><th data-hidden>network passphrases</th></tr></thead><tbody><tr><td>Mainnet</td><td>✅</td><td>✅</td><td>✅</td><td>✅</td><td>✅</td><td><pre><code>Public Global Stellar Network ; September 2015
</code></pre></td></tr></tbody></table>

## Quickstart

In this section, you will learn how to make your first call with either:

* Axios
* Python

### Quickstart with Axios

{% stepper %}
{% step %}
### Set up project

Create and initialize your project:

```bash
mkdir stellar-api-quickstart
cd stellar-api-quickstart
npm init --yes
```
{% endstep %}

{% step %}
### Install Axios

```bash
npm install axios
```
{% endstep %}

{% step %}
### Create index.js

Create a new file named `index.js`. Ensure your `package.json` has `"type": "module"` to use ES modules.
{% endstep %}

{% step %}
### Add the code

Add the following to `index.js`:

```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getHealth",
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
{% endstep %}

{% step %}
### Run the script

```bash
node index.js
```

Expected output:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "status": "healthy",
        "latestLedger": 51583040,
        "oldestLedger": 51565760,
        "ledgerRetentionWindow": 17281
    }
}
```
{% endstep %}
{% endstepper %}

### Quickstart with Python and Requests

{% stepper %}
{% step %}
### Set up project directory

```bash
mkdir stellar-api-quickstart
cd stellar-api-quickstart
```
{% endstep %}

{% step %}
### Create and activate a virtual environment

```bash
python -m venv venv
source venv/bin/activate

# On Windows, use venv\Scripts\activate
```
{% endstep %}

{% step %}
### Install requests

```bash
pip install requests
```
{% endstep %}

{% step %}
### Create main.py

Create a file called `main.py` with the following content:

```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getHealth",
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
{% endstep %}

{% step %}
### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}

## Available API Methods

GetBlock provides access to Stellar RPC methods for interacting with the network:

### Node Information

| Method         | Description                   |
| -------------- | ----------------------------- |
| getHealth      | Returns node health status    |
| getNetwork     | Returns network configuration |
| getVersionInfo | Returns version information   |
| getFeeStats    | Returns fee statistics        |

### Ledger Methods

| Method           | Description                     |
| ---------------- | ------------------------------- |
| getLatestLedger  | Returns the latest known ledger |
| getLedgers       | Returns a list of ledgers       |
| getLedgerEntries | Returns ledger entries by key   |

### Transaction Methods

| Method              | Description                                |
| ------------------- | ------------------------------------------ |
| getTransaction      | Returns transaction details by hash        |
| getTransactions     | Returns a list of transactions             |
| sendTransaction     | Submits a signed transaction               |
| simulateTransaction | Simulates a transaction without submitting |

### Events

| Method    | Description             |
| --------- | ----------------------- |
| getEvents | Returns contract events |

## Support

For technical support and questions:

* Support: support@getblock.io

## See Also

* [Stellar Developer Documentation](https://developers.stellar.org/)
* [Stellar Laboratory](https://laboratory.stellar.org/)
* [Stellar Expert Explorer](https://stellar.expert/)
* [Stellar Dashboard](https://dashboard.stellar.org/)
* [Soroban Smart Contracts](https://soroban.stellar.org/)
