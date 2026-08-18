---
description: >-
  GetBlock provides fast and reliable access to Robinhood nodes via the JSON-RPC
  API. Connect to the Robinhood network without running your own infrastructure.
---

# Robinhood

Robinhood Chain is a permissionless, AI-native Ethereum Layer 2 built on **Arbitrum Orbit** and operated by Robinhood. It launched its public mainnet on July 1, 2026, in London at Robinhood's "The World is Flat" keynote, following a public testnet that opened February 10, 2026, and recorded \~4 million transactions in its first week.

The chain is purpose-built for **real-world assets (RWA)** and **tokenized equities** — its flagship product is Stock Tokens, ERC-20 tokens tracking listed equities (NVDA, AAPL, GOOG, and many more) that trade 24/7, are self-custody-capable, and plug directly into DeFi as collateral. Day-one liquidity comes from a working DeFi stack: Uniswap (dedicated AMM), Lighter (perps), 1inch, Rialto, Arcus, and Pleiades, with Chainlink providing oracles and CCIP interoperability, BitGo providing institutional custody, and Alchemy providing RPC infrastructure. Within its first week, the chain processed roughly 4 million transactions, gathered over $240 million in deposits, and produced \~$570 million of day-one Stock Token trading volume.

Robinhood Chain is fully EVM-compatible — any wallet, contract, or tooling that supports Ethereum works unmodified. The native gas token is **ETH** (no custom chain token to acquire), and the underlying execution client is the standard Arbitrum Nitro stack (Rust-based Reth for execution, Nitro sequencer for ordering). What makes Robinhood Chain distinct is not the execution surface — it's the same as any other Arbitrum Orbit chain — but rather the 100-millisecond **block time**, the flagship RWA use case, and the direct distribution channel through the Robinhood Wallet's 120+ country footprint.

### Key Features

* **100ms Block Time**: soft confirmations at 100 milliseconds — 20x faster than Base's 2-second blocks and 120x faster than Ethereum L1. Enables trading-grade latency for tokenized equities and DeFi
* **Arbitrum Orbit L2**: built on Arbitrum's Nitro stack with Ethereum blobs (EIP-4844) for data availability. Inherits Ethereum's settlement guarantees while offloading execution to a dedicated rollup
* **ETH as Native Gas Token**: no custom chain token required — pay transaction fees in ETH exactly as on Ethereum mainnet. Standard bridging via Arbitrum's canonical bridge or partner routes
* **Full EVM Compatibility**: every Ethereum tool works — ethers.js, viem, Hardhat, Foundry, MetaMask, WalletConnect. Solidity contracts deploy without modification. `web3.py`, `alloy-rs`, and every mainstream JSON-RPC client are compatible out of the box
* **Stock Tokens (ERC-20-Compatible RWAs)**: tokenized equities issued by Robinhood Assets (Jersey) Limited that track listed securities. ERC-20-compatible interface with additional compliance and access-control logic. Standard `balanceOf`, `totalSupply`, `Transfer` event reads all work — no custom methods required
* **First-Come, First-Served Sequencing**: transaction ordering strictly by arrival time at the sequencer. Simpler MEV surface than priority-fee-ordered chains — no priority-gas-auction between sequencer receipt and inclusion
* **Blockscout Explorer**: block exploration via [robinhoodchain.blockscout.com](https://robinhoodchain.blockscout.com/). Etherscan does not index chain ID 4663 — contract verification and transaction views happen exclusively on Blockscout
* **Chainlink-Native**: Chainlink Data Feeds, Data Streams, and CCIP are the official oracle and cross-chain infrastructure for Robinhood-issued assets from day one. No custom oracle setup required for standard price feeds
* **AI-Native Design**: designed to support AI agents that can autonomously trade, swap, lend, and transact with tokenized real-world assets. Complements Robinhood's Agentic Trading and Agentic Accounts products
* **Institutional-Grade Infrastructure**: launch partners include Alchemy (RPC), BitGo (custody), TRM (compliance/AML), LayerZero (cross-chain), and Allium (data). Reliability and compliance built into the day-one stack

{% hint style="info" %}
_**TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC SPECIFICATION**_

_GetBlock's Robinhood Chain API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specifications for the Robinhood Chain JSON-RPC interface — including the standard Ethereum method surface inherited from Arbitrum Nitro and any Arbitrum-Orbit-specific methods (`arb_*`, `arbtrace_*`) — are maintained by:_

* _**Robinhood Chain**:_ [_docs.robinhood.com/chain_](https://docs.robinhood.com/chain)
* _**Arbitrum Nitro**:_ [_docs.arbitrum.io_](https://docs.arbitrum.io/) _— for the underlying Nitro-specific methods and extended block fields_
* _**Standard Ethereum JSON-RPC**:_ [_ethereum.org/en/developers/docs/apis/json-rpc_](https://ethereum.org/en/developers/docs/apis/json-rpc/)
{% endhint %}

### Network Information

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Network Name</td><td>Robinhood Chain</td></tr><tr><td>Chain ID</td><td>4663 (<code>0x1237</code>)</td></tr><tr><td>Native Currency</td><td>ETH</td></tr><tr><td>Decimals</td><td>18</td></tr><tr><td>Block Time</td><td>~100ms (soft confirmations)</td></tr><tr><td>Consensus</td><td>Arbitrum Nitro, first-come-first-served sequencer</td></tr><tr><td>Data Availability</td><td>Ethereum blobs (EIP-4844)</td></tr><tr><td>Framework</td><td>Arbitrum Orbit (Nitro stack)</td></tr><tr><td>Mainnet Launch</td><td>July 1, 2026</td></tr><tr><td>Testnet Launch</td><td>February 10, 2026</td></tr><tr><td>Testnet Chain ID</td><td>46630 (<code>0xB626</code>)</td></tr><tr><td>Block Explorer</td><td><a href="https://robinhoodchain.blockscout.com/">robinhoodchain.blockscout.com</a></td></tr></tbody></table>

### Base URL

{% tabs %}
{% tab title="New York, USA" %}
```bash
https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
All Robinhood Chain JSON-RPC methods are called by sending a `POST` request to the base URL with a JSON-RPC 2.0 body. For real-time subscriptions (`eth_subscribe` / `eth_unsubscribe`), use the WebSocket transport: `wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`.
{% endhint %}

### Supported Networks

| Network                 | JSON-RPC | WSS | New York, USA |
| ----------------------- | -------- | --- | ------------- |
| Robinhood Chain Mainnet | ✅        | ✅   | ✅             |

### Quickstart

In this section, you will learn how to make your first call with either:

* **Axios** (JavaScript / Node.js)
* **Python** (Requests library)

Before you begin, you must have already installed [`npm`](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [`yarn`](https://classic.yarnpkg.com/lang/en/docs/install) on your local machine (for the Axios example) or Python and pip (for the Python example).

{% tabs %}
{% tab title="JavaScript (Axios)" %}
{% stepper %}
{% step %}
#### Setup project

```bash
mkdir robinhood-api-quickstart
cd robinhood-api-quickstart
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
    url: 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/',
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

Replace `<ACCESS-TOKEN>` with your actual GetBlock access token.
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
    "result": "0x1237"
}
```

`0x1237` is `4663` in decimal — Robinhood Chain mainnet's chain ID. This confirms you are connected to the correct network.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% stepper %}
{% step %}
#### Set up the project directory

```bash
mkdir robinhood-api-quickstart
cd robinhood-api-quickstart
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

{% code title="main.py" %}
```python
import requests
import json

url = "https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/"

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

Replace `<ACCESS-TOKEN>` with your actual GetBlock access token.
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

### Available API Methods

Robinhood Chain exposes the full standard Ethereum JSON-RPC surface (inherited from Arbitrum Nitro) plus Arbitrum-Orbit-specific methods for L1 confirmation tracking. Total: 40 methods across 4 namespaces.

#### Web3 Module (2 methods)

| Method              | Description                                             |
| ------------------- | ------------------------------------------------------- |
| web3\_clientVersion | Returns the underlying node client software and version |
| web3\_sha3          | Returns the Keccak-256 hash of the given data           |

#### Net Module (3 methods)

| Method         | Description                                                         |
| -------------- | ------------------------------------------------------------------- |
| net\_version   | Returns the current network ID (`4663` for Robinhood Chain mainnet) |
| net\_listening | Returns `true` if the client is listening for P2P connections       |
| net\_peerCount | Returns the number of currently connected peers                     |

#### Eth Module — Read Methods (22 methods)

| Method                                   | Description                                                                      |
| ---------------------------------------- | -------------------------------------------------------------------------------- |
| eth\_chainId                             | Returns the chain ID (`0x1237` = 4663 for Robinhood Chain mainnet)               |
| eth\_blockNumber                         | Returns the current block number (\~100ms cadence)                               |
| eth\_gasPrice                            | Returns the current gas price in wei (paid in ETH)                               |
| eth\_maxPriorityFeePerGas                | Returns the suggested priority fee (EIP-1559)                                    |
| eth\_feeHistory                          | Returns historical gas data for fee market analysis (EIP-1559)                   |
| eth\_syncing                             | Returns sync status or `false` if fully synced                                   |
| eth\_accounts                            | Returns accounts owned by the client (typically empty on shared nodes)           |
| eth\_getBalance                          | Returns the ETH balance of an address in wei                                     |
| eth\_getCode                             | Returns the bytecode at a contract address                                       |
| eth\_getStorageAt                        | Returns the value stored at a specific storage slot                              |
| eth\_getTransactionCount                 | Returns the nonce for an address                                                 |
| eth\_getBlockByHash                      | Returns block info by hash (may include Arbitrum-specific `l1BlockNumber` field) |
| eth\_getBlockByNumber                    | Returns block info by number (or `latest`, `safe`, `finalized`)                  |
| eth\_getBlockTransactionCountByHash      | Returns the number of transactions in a block by hash                            |
| eth\_getBlockTransactionCountByNumber    | Returns the number of transactions in a block by number                          |
| eth\_getUncleCountByBlockHash            | Returns the uncle count of a block by hash (always 0 on Robinhood Chain)         |
| eth\_getUncleCountByBlockNumber          | Returns the uncle count of a block by number (always 0)                          |
| eth\_getTransactionByHash                | Returns transaction details by hash                                              |
| eth\_getTransactionByBlockHashAndIndex   | Returns transaction by block hash + index                                        |
| eth\_getTransactionByBlockNumberAndIndex | Returns transaction by block number + index                                      |
| eth\_getTransactionReceipt               | Returns the receipt of a mined transaction                                       |
| eth\_getLogs                             | Returns logs matching filter criteria                                            |

#### Eth Module — Execution and State (3 methods)

| Method                | Description                                                          |
| --------------------- | -------------------------------------------------------------------- |
| eth\_call             | Executes a read-only contract call without creating a transaction    |
| eth\_estimateGas      | Estimates gas needed to execute a transaction                        |
| eth\_createAccessList | Creates an EIP-2930 access list for a transaction (gas optimization) |

#### Eth Module — Filters (6 methods)

| Method                           | Description                                              |
| -------------------------------- | -------------------------------------------------------- |
| eth\_newFilter                   | Creates a log filter that subsequent calls can poll      |
| eth\_newBlockFilter              | Creates a filter for new block headers                   |
| eth\_newPendingTransactionFilter | Creates a filter for pending transactions in the mempool |
| eth\_uninstallFilter             | Removes a previously created filter                      |
| eth\_getFilterChanges            | Polls for changes since the last call                    |
| eth\_getFilterLogs               | Returns all logs matching a filter (log filters only)    |

#### Eth Module — Write (1 method)

| Method                  | Description                                 |
| ----------------------- | ------------------------------------------- |
| eth\_sendRawTransaction | Submits a signed transaction to the network |

#### Eth Module — Subscriptions (2 methods, WSS only)

| Method           | Description                                                                    |
| ---------------- | ------------------------------------------------------------------------------ |
| eth\_subscribe   | Subscribes to events — `newHeads`, `logs`, `newPendingTransactions`, `syncing` |
| eth\_unsubscribe | Cancels an existing subscription by ID                                         |

#### Arb Module (1 method, Arbitrum-Orbit-specific)

| Method                  | Description                                                                                                                     |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| arb\_getL1Confirmations | Returns the number of L1 block confirmations for a given L2 block hash — Arbitrum-canonical, used to track L1 settlement status |

### Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

### See Also

* [Official Robinhood Chain Documentation](https://docs.robinhood.com/chain)
* [Robinhood Chain Website](https://robinhood.com/us/en/chain/)
* [Blockscout Block Explorer](https://robinhoodchain.blockscout.com/)
* [Robinhood Chain Launch Announcement (July 1, 2026)](https://robinhood.com/us/en/newsroom/)
* [Chainlink on Robinhood Chain](https://chain.link/case-studies/robinhood-chain)
* [Arbitrum Orbit Documentation](https://docs.arbitrum.io/launch-orbit-chain/orbit-gentle-introduction)
* [Arbitrum Nitro JSON-RPC Reference](https://docs.arbitrum.io/build-decentralized-apps/reference/node-rpc-methods)
* [Standard Ethereum JSON-RPC Reference](https://ethereum.org/en/developers/docs/apis/json-rpc/) (for the inherited `eth_*` surface)
* [Uniswap on Robinhood Chain](https://app.uniswap.org/)
