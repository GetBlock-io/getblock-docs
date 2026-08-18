---
description: >-
  GetBlock provides fast and reliable access to Ronin nodes via the JSON-RPC
  API. Connect to the Ronin network without running your own infrastructure.
---

# Ronin (RON)

Ronin is an EVM-compatible Layer 1 blockchain built by Sky Mavis, purpose-built for Web3 gaming. It is the chain behind Axie Infinity and Pixels, tuned for high-throughput, low-fee onchain gameplay with fast finality. Ronin is EVM-compatible, so Ethereum contracts and tooling — Hardhat, Foundry, ethers.js, viem, and web3.py — run without modification. The native token **RON** is used for gas, staking, and governance.

Ronin secures the chain with a Delegated Proof of Stake (DPoS) consensus operated by a validator set, targeting roughly 3-second blocks and sub-cent fees. This documentation targets Ronin Mainnet (Chain ID `2020`).

## Key Features

* **Gaming-First Layer 1**: Purpose-built by Sky Mavis for Web3 games such as Axie Infinity and Pixels
* **Full EVM Compatibility**: Standard Ethereum JSON-RPC method set; deploy Solidity contracts and use Hardhat, Foundry, ethers.js, viem, and web3.py without modification
* **Delegated Proof of Stake**: A validator set secures the chain with fast finality and roughly 3-second blocks
* **Low Fees, High Throughput**: Sub-cent fees and hundreds of transactions per second for microtransactions
* **RON Native Token**: RON pays gas and is used for staking and governance; WRON is the wrapped ERC-20 form
* **Ronin Bridge**: Moves assets between Ronin and Ethereum
* **Katana DEX and NFT Ecosystem**: A native DeFi and NFT ecosystem around the games it hosts

{% hint style="info" %}
_**TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC SPECIFICATION**_

_GetBlock's Ronin API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for the Ethereum JSON-RPC API is maintained by the Ethereum community and published at_ [_ethereum.org/en/developers/docs/apis/json-rpc/_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_. For Ronin-specific protocol details, consult the official documentation at_ [_docs.roninchain.com_](https://docs.roninchain.com/)_._
{% endhint %}

{% hint style="info" %}
Ronin wallets and explorers commonly display addresses with a `ronin:` prefix instead of `0x` (for example, `ronin:e514...` is the same address as `0xe514...`). The JSON-RPC API uses the standard `0x` hex form; convert `ronin:` addresses to `0x` before sending them in requests.
{% endhint %}

## Network Information

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Network Name</td><td>Ronin Mainnet</td></tr><tr><td>Stage</td><td>Mainnet (live)</td></tr><tr><td>Chain ID</td><td>2020 (<code>0x7e4</code>)</td></tr><tr><td>Native Currency</td><td>RON</td></tr><tr><td>Decimals</td><td>18</td></tr><tr><td>Consensus</td><td>Delegated Proof of Stake (DPoS)</td></tr><tr><td>Smart Contract VM</td><td>EVM (EVM-compatible)</td></tr><tr><td>Address Format</td><td>Ethereum-style (<code>0x…</code>); often shown with a <code>ronin:</code> prefix</td></tr><tr><td>Block Explorer</td><td><a href="https://app.roninchain.com/explorer">app.roninchain.com/explorer</a></td></tr><tr><td>Testnet</td><td>Saigon Testnet (Chain ID 202601)</td></tr></tbody></table>

## Base URL

```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```

All Ronin JSON-RPC methods are called by sending a `POST` request to the base URL with a standard JSON-RPC 2.0 body. For a WebSocket connection, use the WebSocket scheme: `wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`.

## Supported Networks

| Network | JSON-RPC | WSS | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | -------- | --- | ------------------ | ------------- | -------------------- |
| Mainnet | ✅        | ✅   | ✅                  | ✅             | ✅                    |
| Testnet | ✅        | ✅   | ✅                  | ✅             | ✅                    |

Saigon Testnet (Chain ID `202601`) is the test environment for staging contracts before mainnet. Confirm testnet endpoint availability from the GetBlock dashboard.

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
mkdir ronin-api-quickstart
cd ronin-api-quickstart
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

{% code title="index.js" overflow="wrap" %}
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
    "result": "0x7e4",
    "id": "getblock.io"
}
```

The `result` field is the Ronin Mainnet chain ID (`0x7e4` = 2020 in decimal). This confirms you are connected to the correct network.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% stepper %}
{% step %}
#### Setup the project directory

```bash
mkdir ronin-api-quickstart
cd ronin-api-quickstart
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

Ronin exposes the standard Ethereum JSON-RPC method set. All methods are POST-only and called against the base URL above.

### Block & Chain Information

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>eth_blockNumber</td><td>Returns the current latest block number</td></tr><tr><td>eth_chainId</td><td>Returns the chain ID (<code>0x7e4</code> = 2020 for Ronin Mainnet)</td></tr><tr><td>eth_getBlockByNumber</td><td>Returns block information by block number</td></tr><tr><td>eth_getBlockByHash</td><td>Returns block information by block hash</td></tr><tr><td>eth_getBlockTransactionCountByNumber</td><td>Returns the number of transactions in a block by block number</td></tr><tr><td>eth_getBlockTransactionCountByHash</td><td>Returns the number of transactions in a block by block hash</td></tr><tr><td>eth_getBlockReceipts</td><td>Returns all transaction receipts for a given block</td></tr><tr><td>eth_syncing</td><td>Returns sync status or <code>false</code> if the node is fully synced</td></tr></tbody></table>

### Account & State

| Method                   | Description                                           |
| ------------------------ | ----------------------------------------------------- |
| eth\_getBalance          | Returns the native RON balance of an account (in wei) |
| eth\_getCode             | Returns the contract bytecode at a given address      |
| eth\_getStorageAt        | Returns the value at a specific storage slot          |
| eth\_getTransactionCount | Returns the transaction count (nonce) for an account  |

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

| Method              | Description                                       |
| ------------------- | ------------------------------------------------- |
| net\_version        | Returns the network ID (`2020` for Ronin Mainnet) |
| web3\_clientVersion | Returns the client software version               |

## Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Official Ronin Documentation](https://docs.roninchain.com/)
* [Ronin Homepage](https://roninchain.com/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [Ronin Block Explorer](https://app.roninchain.com/explorer)
* [Ronin on Chainlist](https://chainlist.org/chain/2020)
* [Ethers.js Documentation](https://docs.ethers.org/)
* [Viem Documentation](https://viem.sh/)
