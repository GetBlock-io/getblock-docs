---
description: >-
  GetBlock provides fast and reliable access to Sonic nodes via JSON-RPC API.
  Connect to the Sonic network without running your own infrastructure.
---

# Sonic (S)

**Sonic, previously known as Fantom(FTM),** is a high-performance, EVM-compatible Layer 1 blockchain built by Sonic Labs, the team behind the former Fantom network. It pairs a proof-of-stake consensus with full EVM compatibility, so Ethereum contracts and tooling run unmodified, and targets high transaction throughput with **sub-second finality**. The native token S is used for gas, staking, running validators, and governance.

Sonic is designed for responsive DeFi and consumer applications: it retains the Ethereum developer experience while adding a Fee Monetization program that returns a share of network fees to the applications that generate them, and a native Sonic Gateway that bridges liquidity to and from Ethereum. This documentation targets Sonic Mainnet (Chain ID `146`).

## Key Features

* **Full EVM Compatibility**: Standard Ethereum JSON-RPC method set; deploy Solidity contracts and use Hardhat, Foundry, ethers.js, viem, and web3.py without modification
* **Sub-Second Finality**: Transactions reach irreversible finality in under a second
* **Proof-of-Stake Consensus**: Validators secure the network by staking the native S token
* **High Throughput**: Engineered for high transaction volume with low, predictable fees
* **Fee Monetization (FeeM)**: Developers can earn up to 90% of the fees the applications they deploy generate
* **Sonic Gateway**: A native, secure bridge for moving assets and liquidity between Sonic and Ethereum
* **Pectra Compatibility**: Supports the Ethereum Pectra upgrade feature set for up-to-date EVM behavior
* **S Token Utility**: The native S token pays gas and is used for staking, running validators, and governance

{% hint style="info" %}
_**TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC SPECIFICATION**_

_GetBlock's Sonic API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for the Ethereum JSON-RPC API is maintained by the Ethereum community and published at_ [_ethereum.org/en/developers/docs/apis/json-rpc/_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_. For Sonic-specific protocol details and consensus parameters, consult the official documentation at_ [_docs.soniclabs.com_](https://docs.soniclabs.com/)_._
{% endhint %}

## Network Information

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Network Name</td><td>Sonic Mainnet</td></tr><tr><td>Stage</td><td>Mainnet (live)</td></tr><tr><td>Chain ID</td><td>146 (<code>0x92</code>)</td></tr><tr><td>Native Currency</td><td>S</td></tr><tr><td>Decimals</td><td>18</td></tr><tr><td>Consensus</td><td>Proof of Stake (sub-second finality)</td></tr><tr><td>Smart Contract VM</td><td>EVM (Ethereum Virtual Machine)</td></tr><tr><td>EVM Compatible</td><td>Yes (fully compatible)</td></tr><tr><td>Address Format</td><td>Ethereum-style (<code>0x…</code>, 20 bytes)</td></tr><tr><td>Block Explorer</td><td><a href="https://sonicscan.org/">sonicscan.org</a></td></tr><tr><td>Testnet</td><td>Sonic Blaze Testnet (Chain ID 57054)</td></tr></tbody></table>

## Base URL

```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```

{% hint style="info" %}
All Sonic JSON-RPC methods are called by sending a `POST` request to the base URL with a standard JSON-RPC 2.0 body. For a WebSocket connection, use the WebSocket scheme: `wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`.
{% endhint %}

## Supported Networks

| Network | JSON-RPC | WSS | Frankfurt, Germany |
| ------- | -------- | --- | ------------------ |
| Mainnet | ✅        | ✅   | ✅                  |

Sonic Blaze Testnet (Chain ID `57054`) is the test environment for staging contracts before mainnet. Confirm testnet endpoint availability from the GetBlock dashboard.

## Quickstart

In this section, you will learn how to make your first call with either:

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
mkdir sonic-api-quickstart
cd sonic-api-quickstart
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
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_chainId",
    "params": [],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
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
#### Run the script

```bash
node index.js
```

Expected output:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x92"
}
```

The `result` field is the Sonic Mainnet chain ID (`0x92` = 146 in decimal). This confirms you are connected to the correct network.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% stepper %}
{% step %}
#### Setup the project directory

```bash
mkdir sonic-api-quickstart
cd sonic-api-quickstart
```
{% endstep %}

{% step %}
#### Create and activate a virtual environment

```bash
python -m venv venv
source venv/bin/activate

# On Windows, use venv\Scripts\activate
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

{% code title="main.py" %}
```python
import requests
import json

url = "https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_chainId",
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
#### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

## Available API Methods

Sonic exposes the standard Ethereum JSON-RPC method set. All methods are POST-only and called against the base URL above.

### Block & Chain Information

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>eth_blockNumber</td><td>Returns the current latest block number</td></tr><tr><td>eth_chainId</td><td>Returns the chain ID (<code>0x92</code> = 146 for Sonic Mainnet)</td></tr><tr><td>eth_getBlockByNumber</td><td>Returns block information by block number</td></tr><tr><td>eth_getBlockByHash</td><td>Returns block information by block hash</td></tr><tr><td>eth_getBlockTransactionCountByNumber</td><td>Returns the number of transactions in a block by block number</td></tr><tr><td>eth_getBlockTransactionCountByHash</td><td>Returns the number of transactions in a block by block hash</td></tr><tr><td>eth_getBlockReceipts</td><td>Returns all transaction receipts for a given block</td></tr><tr><td>eth_syncing</td><td>Returns sync status or <code>false</code> if the node is fully synced</td></tr></tbody></table>

### Account & State

| Method                   | Description                                          |
| ------------------------ | ---------------------------------------------------- |
| eth\_getBalance          | Returns the native S balance of an account (in wei)  |
| eth\_getCode             | Returns the contract bytecode at a given address     |
| eth\_getStorageAt        | Returns the value at a specific storage slot         |
| eth\_getTransactionCount | Returns the transaction count (nonce) for an account |

### Transactions

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>eth_sendRawTransaction</td><td>Broadcasts a signed transaction</td></tr><tr><td>eth_getTransactionByHash</td><td>Returns a transaction by its hash</td></tr><tr><td>eth_getTransactionByBlockHashAndIndex</td><td>Returns a transaction by block hash and index</td></tr><tr><td>eth_getTransactionByBlockNumberAndIndex</td><td>Returns a transaction by block number and index</td></tr><tr><td>eth_getTransactionReceipt</td><td>Returns the receipt for a transaction by hash</td></tr><tr><td>eth_call</td><td>Executes a read-only call against contract state</td></tr><tr><td>eth_estimateGas</td><td>Estimates gas required for a transaction</td></tr></tbody></table>

### Gas & Fee Market

| Method                    | Description                                               |
| ------------------------- | --------------------------------------------------------- |
| eth\_gasPrice             | Returns the current gas price in wei                      |
| eth\_maxPriorityFeePerGas | Returns the suggested max priority fee per gas (EIP-1559) |
| eth\_feeHistory           | Returns historical base fees and priority fees            |

### Logs

| Method       | Description                          |
| ------------ | ------------------------------------ |
| eth\_getLogs | Returns logs matching a given filter |

### Debug & Trace

| Method                    | Description                                          |
| ------------------------- | ---------------------------------------------------- |
| debug\_traceTransaction   | Replays a transaction and returns an execution trace |
| debug\_traceBlockByNumber | Traces every transaction in a block by block number  |
| debug\_traceBlockByHash   | Traces every transaction in a block by block hash    |

### Network & Client Info

| Method              | Description                                      |
| ------------------- | ------------------------------------------------ |
| net\_version        | Returns the network ID (`146` for Sonic Mainnet) |
| web3\_clientVersion | Returns the client software version              |

## Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Official Sonic Documentation](https://docs.soniclabs.com/)
* [Sonic Homepage](https://www.soniclabs.com/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [Sonic Block Explorer (SonicScan)](https://sonicscan.org/)
* [Sonic Blaze Testnet Faucet](https://testnet.soniclabs.com/account)
* [Sonic on Chainlist](https://chainlist.org/chain/146)
* [Ethers.js Documentation](https://docs.ethers.org/)
* [Viem Documentation](https://viem.sh/)
