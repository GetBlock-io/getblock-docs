---
description: >-
  GetBlock provides fast and reliable access to Celo nodes via JSON-RPC API.
  Connect to the Celo network without running your own infrastructure.
---

# Celo

Celo is a mobile-first, carbon-negative Ethereum Layer 2 blockchain designed to make financial tools accessible to anyone with a smartphone. Originally launched as an EVM-compatible Layer 1 in 2020, Celo completed its transition to an Ethereum L2 using the OP Stack in March 2025. The network features 1-second block times, one-block finality, sub-cent transaction fees, and the unique ability to pay gas fees in stablecoins (cUSD, cEUR, USDC, USDT) in addition to CELO.

### Key Features

* **Ethereum Layer 2**: Built on OP Stack, leveraging Ethereum's security with fast, low-cost transactions
* **Mobile-First Design**: Ultralight clients (Plumo) enable blockchain access on low-powered smartphones
* **Fee Abstraction**: Pay transaction fees in CELO or stablecoins (cUSD, cEUR, cREAL, USDC, USDT)
* **Phone Number Identity**: Map phone numbers to wallet addresses for easy payments
* **Native Stablecoins**: cUSD, cEUR, and cREAL via the Mento Protocol for stable payments
* **One-Block Finality**: \~1 second block times with instant transaction finality
* **Carbon-Negative**: First carbon-negative blockchain through on-chain offsets
* **Full EVM Compatibility**: Deploy Ethereum smart contracts without modification
* **CELO Token**: Native token for gas fees, staking, and governance (18 decimals)

{% hint style="info" %}
TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION.

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and streamlined developer experience optimization. The canonical and normative specification for Ethereum Virtual Machine (EVM) JSON-RPC methods is solely maintained and published through the official Ethereum Foundation documentation portal at_ [_ethereum.org_](http://ethereum.org/)_. This resource constitutes the sole authoritative reference implementation of the JSON-RPC 2.0 protocol interface across EVM-compatible execution clients._
{% endhint %}

## Network Information

| Property        | Value                       |
| --------------- | --------------------------- |
| Network Name    | Celo Mainnet                |
| Chain ID        | 42220                       |
| Native Currency | CELO                        |
| Decimals        | 18                          |
| RPC URL         | https://forno.celo.org      |
| Network Type    | Ethereum Layer 2 (OP Stack) |
| Block Time      | \~1 second                  |
| Finality        | One-block finality          |

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

| Network | Chain ID | JSON RPC | WSS | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | -------- | -------- | --- | ------------------ | ------------- | -------------------- |
| Mainnet | 42220    | ✅        | ✅   | ✅                  | ✅             | ✅                    |
| Sepolia | 11142220 | ✅        | ✅   | ✅                  | ✅             | ✅                    |

## Quickstart

In this section, you will learn how to make your first call with either:

* Axios
* Python

Before you begin, you must have already installed [`npm`](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [`yarn`](https://classic.yarnpkg.com/lang/en/docs/install) on your local machine (for the Axios example) or Python and pip (for the Python example).

### Quickstart with Axios

{% stepper %}
{% step %}
### Setup project

Create and initialize a new project:

```bash
mkdir celo-api-quickstart
cd celo-api-quickstart
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
    "method": "eth_blockNumber",
    "params": [],
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
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x1d9f2a8"
}
```
{% endstep %}
{% endstepper %}

### Quickstart with Python and Requests

{% stepper %}
{% step %}
### Setup the project directory

```bash
mkdir celo-api-quickstart
cd celo-api-quickstart
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
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

## Available API Methods

GetBlock provides access to standard Ethereum JSON-RPC methods for the Celo network.

#### Transaction Methods

| Method                                   | Description                                   |
| ---------------------------------------- | --------------------------------------------- |
| eth\_getTransactionByHash                | Returns transaction by hash                   |
| eth\_getTransactionByBlockHashAndIndex   | Returns transaction by block hash and index   |
| eth\_getTransactionByBlockNumberAndIndex | Returns transaction by block number and index |
| eth\_getTransactionCount                 | Returns the transaction count (nonce)         |
| eth\_getTransactionReceipt               | Returns the receipt of a transaction          |
| eth\_sendRawTransaction                  | Submits a signed transaction                  |

#### Block Methods

| Method                                | Description                               |
| ------------------------------------- | ----------------------------------------- |
| eth\_blockNumber                      | Returns the current block number          |
| eth\_getBlockByHash                   | Returns block by hash                     |
| eth\_getBlockByNumber                 | Returns block by number                   |
| eth\_getBlockReceipts                 | Returns all receipts for a block          |
| eth\_getBlockTransactionCountByHash   | Returns transaction count by block hash   |
| eth\_getBlockTransactionCountByNumber | Returns transaction count by block number |

#### Account/State Methods

| Method            | Description                                    |
| ----------------- | ---------------------------------------------- |
| eth\_getBalance   | Returns the CELO balance of an address         |
| eth\_getStorageAt | Returns the value at a storage position        |
| eth\_getCode      | Returns the code at an address                 |
| eth\_call         | Executes a call without creating a transaction |
| eth\_getProof     | Returns account and storage proof              |
| eth\_accounts     | Returns a list of addresses owned by client    |

#### Gas and Fee Methods

| Method                    | Description                        |
| ------------------------- | ---------------------------------- |
| eth\_gasPrice             | Returns the current gas price      |
| eth\_estimateGas          | Estimates gas for a transaction    |
| eth\_feeHistory           | Returns historical gas information |
| eth\_maxPriorityFeePerGas | Returns current max priority fee   |

#### Filter Methods

| Method                | Description                            |
| --------------------- | -------------------------------------- |
| eth\_getLogs          | Returns logs matching filter criteria  |
| eth\_newFilter        | Creates a log filter                   |
| eth\_newBlockFilter   | Creates a block filter                 |
| eth\_getFilterChanges | Returns filter changes since last poll |
| eth\_getFilterLogs    | Returns all logs matching filter       |
| eth\_uninstallFilter  | Removes a filter                       |

#### Subscription Methods (WebSocket)

| Method           | Description                       |
| ---------------- | --------------------------------- |
| eth\_subscribe   | Creates a subscription for events |
| eth\_unsubscribe | Cancels an existing subscription  |

#### Network/Chain Methods

| Method              | Description                       |
| ------------------- | --------------------------------- |
| eth\_chainId        | Returns the chain ID (42220)      |
| eth\_syncing        | Returns sync status               |
| net\_version        | Returns the network ID            |
| net\_listening      | Returns listening status          |
| net\_peerCount      | Returns number of connected peers |
| web3\_clientVersion | Returns client version            |
| web3\_sha3          | Returns Keccak-256 hash of data   |

#### Debug Methods

| Method                    | Description                                |
| ------------------------- | ------------------------------------------ |
| debug\_traceBlockByHash   | Traces all transactions in a block by hash |
| debug\_traceBlockByNumber | Traces block by number                     |
| debug\_traceTransaction   | Traces a specific transaction              |
| debug\_traceCall          | Traces a call without creating transaction |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Celo Developer docs](https://docs.celo.org/)
* [Celo Explorer](https://celoscan.io/)
* [Get Celo Faucet](https://faucet.celo.org/)
* [Ethereum JSON RPC API](https://ethereum.org/developers/docs/apis/json-rpc/)
* [Celo Website](https://celo.org/)
