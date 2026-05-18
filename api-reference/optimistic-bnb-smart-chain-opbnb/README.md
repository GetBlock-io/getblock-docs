---
description: >-
  opBNB API Reference for efficient interaction with opBNB nodes, enabling
  scalable, low-cost transactions and fast finality through Layer 2 solutions on
  the BSC blockchain.
---

# Optimistic BNB Smart Chain(opBNB)

opBNB is a high-performance, EVM-compatible Layer 2 scaling solution for BNB Smart Chain (BSC), built on the OP Stack — the open-source rollup framework developed by the team behind Optimism. Launched in August 2023 by the BNB Chain core team, opBNB uses optimistic rollup technology to inherit BSC's security while offering sub-second block times, 4,000+ TPS throughput, and median transaction fees below $0.005. The network is fully compatible with the Ethereum JSON-RPC specification, allowing developers to port any EVM-based dApp with minimal changes.

### Key Features

* **OP Stack Architecture**: Built on the same battle-tested optimistic rollup framework as Optimism and Base, ensuring tooling compatibility and a familiar developer experience
* **Sub-Second Block Times**: \~1 second L2 block production for near-instant transaction confirmations, ideal for gaming, micropayments, and high-frequency dApps
* **High Throughput**: 4,000+ TPS capacity with 100 MB block size — among the highest-throughput EVM chains in production
* **Ultra-Low Fees**: Median fees below $0.005 per transaction, with BNB Greenfield integration on the roadmap to reduce costs by an additional 5–10×
* **Native Token (BNB)**: Gas paid in BNB (18 decimals), bridged 1:1 from BSC — no separate gas token to manage
* **Full EVM Compatibility**: Standard Ethereum JSON-RPC method set; deploy Solidity, Vyper, or any EVM bytecode without modification
* **BSC Settlement**: Settles transaction data to BNB Smart Chain as L1, inheriting BSC's validator security model
* **DeFi & Gaming Focus**: Optimized for high-frequency, low-value transactions — already home to 90+ dApps spanning DeFi, on-chain gaming, and SocialFi
* **Optimistic Rollup Security**: Fraud-proof window with 7-day challenge period; users retain L1-level security guarantees
* **Ecosystem Tooling**: First-class support for Hardhat, Foundry, Truffle, ethers.js, viem, and web3.py through the standard OP Stack toolchain

{% hint style="info" %}
_**TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC SPECIFICATION**_

_GetBlock’s opBNB API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for the Ethereum JSON-RPC API is maintained by the Ethereum community and published at_ [_ethereum.org/en/developers/docs/apis/json-rpc/_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_. For opBNB-specific protocol details, consensus parameters, and ecosystem tooling, consult the official documentation at_ [_docs.bnbchain.org/bnb-opbnb/_](https://docs.bnbchain.org/bnb-opbnb/)_._
{% endhint %}

## Network Information

| Property          | Value                                   |
| ----------------- | --------------------------------------- |
| Network Name      | opBNB Mainnet                           |
| Chain ID          | 204                                     |
| Native Currency   | BNB                                     |
| Decimals          | 18                                      |
| Block Time        | \~1 second                              |
| Consensus         | Optimistic Rollup on OP Stack           |
| Settlement Layer  | BNB Smart Chain (BSC, Chain ID 56)      |
| Smart Contract VM | EVM (Ethereum Virtual Machine)          |
| EVM Compatible    | Yes (fully compatible)                  |
| Address Format    | Ethereum-style (`0x…`, 20 bytes)        |
| Block Explorer    | [opbnbscan.com](https://opbnbscan.com/) |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

All opBNB JSON-RPC methods are called by sending a `POST` request to the base URL with a standard JSON-RPC 2.0 body. There is no separate `/jsonRPC` path or REST `GET` form — the access token in the URL path handles authentication.

## Supported Networks

| Network | JSON-RPC | WSS | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | -------- | --- | ------------------ | ------------- | -------------------- |
| Mainnet | ✅        | ✅   | ✅                  | ✅             | ✅                    |
| Testnet | ✅        | ✅   | ✅                  | ✅             | ✅                    |

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
mkdir opbnb-api-quickstart
cd opbnb-api-quickstart
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

Expected output (example):

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x893e5fa"
}
```

The `result` field is the latest block number in hexadecimal. Convert it to decimal to read the block height.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% stepper %}
{% step %}
#### Setup the project directory

```bash
mkdir opbnb-api-quickstart
cd opbnb-api-quickstart
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
#### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

## Available API Methods

opBNB exposes the full standard Ethereum JSON-RPC method set. All methods are POST-only and called against the base URL above.

### Block & Chain Information

| Method                                | Description                                                   |
| ------------------------------------- | ------------------------------------------------------------- |
| eth\_blockNumber                      | Returns the current latest block number                       |
| eth\_chainId                          | Returns the chain ID (`0xcc` = 204 for opBNB mainnet)         |
| eth\_getBlockByNumber                 | Returns block information by block number                     |
| eth\_getBlockByHash                   | Returns block information by block hash                       |
| eth\_getBlockTransactionCountByNumber | Returns the number of transactions in a block by block number |
| eth\_getBlockTransactionCountByHash   | Returns the number of transactions in a block by block hash   |
| eth\_getBlockReceipts                 | Returns all transaction receipts for a given block            |
| eth\_syncing                          | Returns sync status or `false` if node is fully synced        |

### Account & State

| Method                   | Description                                                       |
| ------------------------ | ----------------------------------------------------------------- |
| eth\_getBalance          | Returns the BNB balance of an account (in wei)                    |
| eth\_getCode             | Returns the contract bytecode at a given address                  |
| eth\_getStorageAt        | Returns the value at a specific storage slot                      |
| eth\_getProof            | Returns the Merkle proof for an account and optional storage keys |
| eth\_getTransactionCount | Returns the transaction count (nonce) for an account              |

### Transactions

| Method                                   | Description                                      |
| ---------------------------------------- | ------------------------------------------------ |
| eth\_sendRawTransaction                  | Broadcasts a signed transaction                  |
| eth\_getTransactionByHash                | Returns a transaction by its hash                |
| eth\_getTransactionByBlockHashAndIndex   | Returns a transaction by block hash and index    |
| eth\_getTransactionByBlockNumberAndIndex | Returns a transaction by block number and index  |
| eth\_getTransactionReceipt               | Returns the receipt for a transaction by hash    |
| eth\_call                                | Executes a read-only call against contract state |
| eth\_estimateGas                         | Estimates gas required for a transaction         |

### Gas & Fee Market

| Method                    | Description                                               |
| ------------------------- | --------------------------------------------------------- |
| eth\_gasPrice             | Returns the current gas price                             |
| eth\_maxPriorityFeePerGas | Returns the suggested max priority fee per gas (EIP-1559) |
| eth\_feeHistory           | Returns historical base fees and priority fees            |

### Logs & Filters

| Method                           | Description                                             |
| -------------------------------- | ------------------------------------------------------- |
| eth\_getLogs                     | Returns logs matching a given filter                    |
| eth\_newFilter                   | Creates a new log filter                                |
| eth\_newBlockFilter              | Creates a filter that fires on new blocks               |
| eth\_newPendingTransactionFilter | Creates a filter that fires on new pending transactions |
| eth\_getFilterChanges            | Polls a filter for new events since the last poll       |
| eth\_getFilterLogs               | Returns all logs matching a filter                      |
| eth\_uninstallFilter             | Uninstalls a filter                                     |

### WebSocket Subscriptions

| Method           | Description                                                         |
| ---------------- | ------------------------------------------------------------------- |
| eth\_subscribe   | Subscribes to events (`newHeads`, `logs`, `newPendingTransactions`) |
| eth\_unsubscribe | Cancels an existing subscription                                    |

### Network & Client Info

| Method              | Description                                                        |
| ------------------- | ------------------------------------------------------------------ |
| net\_version        | Returns the network ID (`204` for opBNB mainnet)                   |
| net\_listening      | Returns `true` if the client is actively listening for connections |
| net\_peerCount      | Returns the number of peers connected to the node                  |
| web3\_clientVersion | Returns the client software version                                |
| web3\_sha3          | Returns the Keccak-256 hash of the given data                      |

## Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Official opBNB Documentation](https://docs.bnbchain.org/bnb-opbnb/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [OP Stack Documentation](https://docs.optimism.io/stack/getting-started)
* [opBNBScan Block Explorer](https://opbnbscan.com/)
* [opBNB on Chainlist](https://chainlist.org/chain/204)
* [Ethers.js Documentation](https://docs.ethers.org/)
* [Viem Documentation](https://viem.sh/)
* [Hardhat Documentation](https://hardhat.org/docs)
