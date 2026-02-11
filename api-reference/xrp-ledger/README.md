---
description: >-
  GetBlock provides fast and reliable access to XRP Ledger nodes via JSON-RPC
  and WebSocket APIs. Connect to the XRP Ledger network without running your own
  infrastructure.
---

# XRP Ledger

XRP Ledger (XRPL) is a decentralized, open-source blockchain launched in 2012. Unlike Bitcoin or Ethereum, XRPL uses a unique Federated Consensus mechanism that enables fast, low-cost transactions without mining. The network processes over 1,500 transactions per second with settlement times of 3-5 seconds, making it ideal for payments, tokenization, and DeFi applications.

### Key Features

* **Federated Consensus Protocol**: Unique consensus mechanism using trusted validators (Unique Node Lists) instead of Proof of Work or Proof of Stake
* **Fast Settlement**: Transactions finalize in 3-5 seconds
* **High Throughput**: Supports 1,500+ transactions per second
* **Low Transaction Fees**: Fees are typically less than $0.01 per transaction
* **Native DEX**: Built-in decentralized exchange for trading any issued tokens
* **Energy Efficient**: Minimal energy consumption compared to PoW blockchains
* **XRP Native Token**: Used for transaction fees, bridging currencies, and as a store of value
* **Token Issuance**: Native support for issuing custom tokens and stablecoins
* **NFT Support**: XLS-20 standard for minting, trading, and burning NFTs
* **No Smart Contracts by Default**: Purpose-built for payments; EVM sidechain available for smart contract functionality

{% hint style="info" %}
**TECHNICAL DISCLAIMER**\
The canonical specification for XRP Ledger API methods is maintained in the official XRPL documentation at [xrpl.org](https://xrpl.org/docs/references/http-websocket-apis/).
{% endhint %}

### Network Configuration

| Property          | Value                       |
| ----------------- | --------------------------- |
| Network Name      | XRP Ledger                  |
| Native Token      | XRP                         |
| Decimals          | 6 (1 XRP = 1,000,000 drops) |
| Ledger Close Time | \~3-5 seconds               |
| Account Reserve   | 10 XRP base reserve         |
| Block Explorer    | https://xrpscan.com         |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network | Chain ID | JSON-RPC | WSS |
| ------- | -------- | -------- | --- |
| Mainnet | 1600     | ✅        | ✅   |

## Quickstart

In this section, you will learn how to make your first call with either:

* **Axios** (JavaScript/Node.js)
* **Python** (Requests library)

Before you begin, you must have already installed `npm` or `yarn` on your local machine (for the Axios example) or Python and pip (for the Python example).

Refer to:

* [npm installation guide](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
* [yarn installation guide](https://classic.yarnpkg.com/lang/en/docs/install)

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

Create and initialize a new project:

```bash
mkdir xrpl-api-quickstart
cd xrpl-api-quickstart
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
### Create file

Create a new file named `index.js`. This is where you will make your first call.
{% endstep %}

{% step %}
### Set ES module type

Set the ES module `"type": "module"` in your `package.json`.
{% endstep %}

{% step %}
### Add code

Add the following code to `index.js`:

{% code title="index.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "server_info",
    "params": [{}],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS_TOKEN>',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
{% endstep %}

{% step %}
### Run the script

```bash
node index.js
```

Expected output (example):

```json
{
    "result": {
        "info": {
            "build_version": "3.1.0",
            "complete_ledgers": "100807399-101925334",
            "hostid": "OVA",
            "initial_sync_duration_us": "714094236",
            "io_latency_ms": 1,
            "jq_trans_overflow": "0",
            "last_close": {
                "converge_time_s": 3.002,
                "proposers": 34
            },
            "load_factor": 1,
            "network_id": 0,
            "peer_disconnects": "5975",
            "peer_disconnects_resources": "0",
            "peers": 131,
            "ports": [
                {
                    "port": "5005",
                    "protocol": [
                        "http"
                    ]
                },
                {
                    "port": "80",
                    "protocol": [
                        "ws"
                    ]
                },
                {
                    "port": "2459",
                    "protocol": [
                        "peer"
                    ]
                },
                {
                    "port": "50051",
                    "protocol": [
                        "grpc"
                    ]
                }
            ],
            "pubkey_node": "n9437mTpq2oNhbBqDPpibuPz11yaTAS76iQNqVTXHF13qDgXgcPB",
            "server_state": "full",
            "server_state_duration_us": "166954693311",
            "state_accounting": {
                "connected": {
                    "duration_us": "708821254",
                    "transitions": "2"
                },
                "disconnected": {
                    "duration_us": "1087525",
                    "transitions": "2"
                },
                "full": {
                    "duration_us": "166954693311",
                    "transitions": "1"
                },
                "syncing": {
                    "duration_us": "4185421",
                    "transitions": "1"
                },
                "tracking": {
                    "duration_us": "33",
                    "transitions": "1"
                }
            },
            "time": "2026-Jan-31 05:36:36.646370 UTC",
            "uptime": 167668,
            "validated_ledger": {
                "age": 4,
                "base_fee_xrp": 1e-05,
                "hash": "2AAEF99EFEDD15D293F8A1023AA873F2D7675FD6A55F5C19BC07C192EEC946B4",
                "reserve_base_xrp": 1,
                "reserve_inc_xrp": 0.2,
                "seq": 101925334
            },
            "validation_quorum": 28
        },
        "status": "success"
    }
}
```
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python(Request)" %}
{% stepper %}
{% step %}
### Setup the project directory

```bash
mkdir xrpl-api-quickstart
cd xrpl-api-quickstart
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
### Create script

Create a file called `main.py` with the following content:

{% code title="main.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS_TOKEN>"

payload = json.dumps({
  "jsonrpc": "2.0",
  "method": "server_info",
  "params": [
    {}
  ],
  "id": "getblock.io"
})
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
{% endstep %}

{% step %}
### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

## Supported Methods

### Account Methods

| Method              | Description                                  |
| ------------------- | -------------------------------------------- |
| account\_info       | Get account information and XRP balance      |
| account\_channels   | Get payment channels where account is source |
| account\_currencies | Get currencies an account can send/receive   |
| account\_lines      | Get trust lines for an account               |
| account\_objects    | Get raw ledger objects owned by account      |
| account\_offers     | Get outstanding offers by account            |
| account\_tx         | Get transactions for an account              |

### Ledger Methods

| Method          | Description                      |
| --------------- | -------------------------------- |
| ledger          | Get ledger information           |
| ledger\_closed  | Get most recently closed ledger  |
| ledger\_current | Get current working ledger       |
| ledger\_data    | Get contents of specified ledger |
| ledger\_entry   | Get specific ledger entry        |

### Transaction Methods

| Method              | Description                          |
| ------------------- | ------------------------------------ |
| tx                  | Get transaction by hash              |
| submit              | Submit a signed transaction          |
| submit\_multisigned | Submit a multi-signed transaction    |
| transaction\_entry  | Get transaction from specific ledger |
| tx\_history         | Get recent transactions (deprecated) |

### Path & Order Book Methods

| Method              | Description                       |
| ------------------- | --------------------------------- |
| book\_offers        | Get offers to exchange currencies |
| deposit\_authorized | Check if deposit is authorized    |
| path\_find          | Find payment paths                |
| ripple\_path\_find  | Find ripple payment paths         |

### Payment Channel Methods

| Method             | Description                            |
| ------------------ | -------------------------------------- |
| channel\_authorize | Authorize payment channel claim        |
| channel\_verify    | Verify payment channel claim signature |

### Subscription Methods

| Method      | Description             |
| ----------- | ----------------------- |
| subscribe   | Subscribe to events     |
| unsubscribe | Unsubscribe from events |

### Server Methods

| Method        | Description                 |
| ------------- | --------------------------- |
| fee           | Get current transaction fee |
| server\_info  | Get server information      |
| server\_state | Get server state            |
| manifest      | Get validator manifest      |
| ping          | Ping the server             |
| random        | Generate random number      |

### Utility Methods

| Method | Description              |
| ------ | ------------------------ |
| json   | Proxy for other commands |

### Signing Methods (Admin)

| Method    | Description              |
| --------- | ------------------------ |
| sign      | Sign a transaction       |
| sign\_for | Sign for multi-signature |

### Special Methods

| Method            | Description                |
| ----------------- | -------------------------- |
| gateway\_balances | Get gateway balances       |
| noripple\_check   | Check NoRipple flag status |

## Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

### See Also

* [XRP Ledger Developer Documentation](https://xrpl.org/docs/)
* [XRP Ledger Block Explorer - XRPScan](https://xrpscan.com/)
* [XRP Ledger Block Explorer - Bithomp](https://bithomp.com/)
* [XRPL Learning Portal](https://learn.xrpl.org/)
* [XRP Ledger API Reference](https://xrpl.org/docs/references/http-websocket-apis/)
* [XRPL JavaScript Library](https://github.com/XRPLF/xrpl.js)
* [XRP Ledger Testnet Faucet](https://xrpl.org/resources/dev-tools/xrp-faucets)
