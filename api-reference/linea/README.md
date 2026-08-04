---
description: >-
  GetBlock provides fast and reliable access to Linea nodes via JSON-RPC API.
  Connect to the Linea network without running your own infrastructure.
---

# Linea

Linea is an Ethereum Layer 2 zkEVM developed by Consensys, the team behind MetaMask and Infura. It executes standard EVM bytecode natively, uses **ETH for gas**, and settles security to Ethereum Layer 1 through zero-knowledge validity proofs. As a Type 2, EVM-equivalent zkEVM, Linea runs existing Ethereum contracts and tooling — Hardhat, Foundry, ethers.js, viem, and web3.py — without modification.

Because Linea settles proofs to Ethereum, it inherits Ethereum's security while offering lower fees and faster soft finality than Layer 1. This documentation targets Linea Mainnet (Chain ID `59144`) and covers the standard Ethereum JSON-RPC method set plus Linea-specific extensions such as `linea_estimateGas` and `linea_getProof`.

## Key Features

* **Full EVM Equivalence (Type 2 zkEVM)**: Runs standard EVM bytecode; deploy Solidity contracts and use Hardhat, Foundry, ethers.js, viem, and web3.py without modification
* **Zero-Knowledge Validity Proofs**: State is proven to Ethereum L1 with zk proofs rather than fraud proofs, avoiding optimistic challenge windows
* **Ethereum Security**: Settles to Ethereum L1 and inherits its security guarantees
* **ETH for Gas**: Transaction fees are paid in ETH, the same asset as Ethereum mainnet
* **Consensys Ecosystem**: First-class integration with MetaMask and Infura tooling
* **Linea Gas Extensions**: `linea_estimateGas` returns an L1-cost-aware gas price for accurate fee estimation
* **State Proof Extension**: `linea_getProof` returns Sparse Merkle Trie proofs for verifying Linea state on L1
* **Ethereum Upgrade Parity**: Tracks Ethereum network upgrades to keep EVM behavior current

{% hint style="info" %}
_**TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC SPECIFICATION**_

_GetBlock's Linea API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for the Ethereum JSON-RPC API is maintained by the Ethereum community and published at_ [_ethereum.org/en/developers/docs/apis/json-rpc/_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_. For Linea-specific methods and protocol details, consult the official documentation at_ [_docs.linea.build_](https://docs.linea.build/)_._
{% endhint %}

## Network Information

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Network Name</td><td>Linea Mainnet</td></tr><tr><td>Stage</td><td>Mainnet (live)</td></tr><tr><td>Chain ID</td><td>59144 (<code>0xe708</code>)</td></tr><tr><td>Native Currency</td><td>ETH</td></tr><tr><td>Decimals</td><td>18</td></tr><tr><td>Layer</td><td>Layer 2 (zkEVM rollup on Ethereum)</td></tr><tr><td>Consensus</td><td>Zero-knowledge validity proofs settled to Ethereum</td></tr><tr><td>Smart Contract VM</td><td>EVM (Type 2, EVM-equivalent)</td></tr><tr><td>Address Format</td><td>Ethereum-style (<code>0x…</code>, 20 bytes)</td></tr><tr><td>Block Explorer</td><td><a href="https://lineascan.build/">lineascan.build</a></td></tr><tr><td>Testnet</td><td>Linea Sepolia (Chain ID 59141)</td></tr></tbody></table>

## Base URL

```bash
https://go.getblock.io/<ACCESS-TOKEN>/
```

{% hint style="info" %}
All Linea JSON-RPC methods are called by sending a `POST` request to the base URL with a standard JSON-RPC 2.0 body. For a WebSocket connection, use the WebSocket scheme: `wss://go.getblock.io/<ACCESS-TOKEN>/`.
{% endhint %}

## Supported Networks

| Network | JSON-RPC | WSS | Graph QL | Frankfurt, Germany |
| ------- | -------- | --- | -------- | ------------------ |
| Mainnet | ✅        | ✅   | ✅        | ✅                  |
| Sepolia | ✅        | ✅   | ✅        | ✅                  |

Linea Sepolia (Chain ID `59141`) is the test environment for staging contracts before mainnet. Confirm testnet endpoint availability from the GetBlock dashboard.

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
mkdir linea-api-quickstart
cd linea-api-quickstart
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
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
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
    "result": "0xe708"
}
```

The `result` field is the Linea Mainnet chain ID (`0xe708` = 59144 in decimal). This confirms you are connected to the correct network.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% stepper %}
{% step %}
#### Setup the project directory

```bash
mkdir linea-api-quickstart
cd linea-api-quickstart
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

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

Linea exposes the standard Ethereum JSON-RPC method set plus Linea-specific extensions. All methods are POST-only and called against the base URL above.

### Block & Chain Information

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>eth_blockNumber</td><td>Returns the current latest block number</td></tr><tr><td>eth_chainId</td><td>Returns the chain ID (<code>0xe708</code> = 59144 for Linea Mainnet)</td></tr><tr><td>eth_getBlockByNumber</td><td>Returns block information by block number</td></tr><tr><td>eth_getBlockByHash</td><td>Returns block information by block hash</td></tr><tr><td>eth_getBlockTransactionCountByNumber</td><td>Returns the number of transactions in a block by block number</td></tr><tr><td>eth_getBlockTransactionCountByHash</td><td>Returns the number of transactions in a block by block hash</td></tr><tr><td>eth_getBlockReceipts</td><td>Returns all transaction receipts for a given block</td></tr><tr><td>eth_syncing</td><td>Returns sync status or <code>false</code> if the node is fully synced</td></tr></tbody></table>

### Account & State

| Method                   | Description                                           |
| ------------------------ | ----------------------------------------------------- |
| eth\_getBalance          | Returns the native ETH balance of an account (in wei) |
| eth\_getCode             | Returns the contract bytecode at a given address      |
| eth\_getStorageAt        | Returns the value at a specific storage slot          |
| eth\_getTransactionCount | Returns the transaction count (nonce) for an account  |

### Transactions

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>eth_sendRawTransaction</td><td>Broadcasts a signed transaction</td></tr><tr><td>eth_getTransactionByHash</td><td>Returns a transaction by its hash</td></tr><tr><td>eth_getTransactionByBlockHashAndIndex</td><td>Returns a transaction by block hash and index</td></tr><tr><td>eth_getTransactionByBlockNumberAndIndex</td><td>Returns a transaction by block number and index</td></tr><tr><td>eth_getTransactionReceipt</td><td>Returns the receipt for a transaction by hash</td></tr><tr><td>eth_call</td><td>Executes a read-only call against contract state</td></tr><tr><td>eth_estimateGas</td><td>Estimates gas required for a transaction</td></tr></tbody></table>

### Gas & Fee Market

| Method                    | Description                                                 |
| ------------------------- | ----------------------------------------------------------- |
| eth\_gasPrice             | Returns the current gas price in wei                        |
| eth\_maxPriorityFeePerGas | Returns the suggested max priority fee per gas (EIP-1559)   |
| eth\_feeHistory           | Returns historical base fees and priority fees              |
| linea\_estimateGas        | Returns an L1-cost-aware gas estimate (gasLimit, base, tip) |

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

| Method              | Description                                        |
| ------------------- | -------------------------------------------------- |
| net\_version        | Returns the network ID (`59144` for Linea Mainnet) |
| web3\_clientVersion | Returns the client software version                |

## Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Official Linea Documentation](https://docs.linea.build/)
* [Linea Homepage](https://linea.build/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [Linea Block Explorer (LineaScan)](https://lineascan.build/)
* [Linea on Chainlist](https://chainlist.org/chain/59144)
* [Ethers.js Documentation](https://docs.ethers.org/)
* [Viem Documentation](https://viem.sh/)
