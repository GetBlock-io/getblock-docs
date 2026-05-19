---
description: >-
  GetBlock provides fast and reliable access to Arc nodes via JSON-RPC API.
  Connect to the ARC network without running your own infrastructure.
---

# ARC

Arc is an open Layer 1 blockchain built by Circle (the company behind USDC), purpose-built for stablecoin finance, payments, and institutional use cases. Launched on public testnet in late 2025 with participation from 100+ institutions, including BlackRock, Visa, and HSBC, Arc takes a fundamentally different approach to gas economics: instead of a volatile native token, **USDC is the network's native gas token**, making transaction fees predictable and dollar-denominated.&#x20;

Arc is fully EVM-compatible, runs on Circle's Malachite consensus engine for deterministic sub-second finality, and ships with a built-in FX engine for 24/7 PvP stablecoin settlement plus opt-in compliance-friendly privacy. The result is a chain optimized for real-world financial activity — cross-border payments, capital markets, eCommerce checkout, and tokenized credit — that retains the developer experience of Ethereum.

## Key Features

* **USDC as Native Gas**: Transaction fees paid in USDC (6 decimals, not 18) — eliminates crypto-volatility from fee budgets and enables dollar-denominated cost predictability
* **Malachite Consensus**: Deterministic sub-second finality engineered for financial infrastructure standards — no probabilistic settlement windows
* **Full EVM Compatibility**: Standard Ethereum JSON-RPC method set; deploy Solidity / Vyper contracts and use Hardhat, Foundry, ethers.js, viem, and web3.py without modification
* **Built-in FX Engine**: Native on-chain currency exchange with 24/7 PvP (Payment versus Payment) settlement for stablecoin pairs
* **Opt-In Privacy**: Compliance-friendly privacy primitives that institutions can selectively enable for confidential financial operations
* **Circle Ecosystem Integration**: First-class support for CCTP (Cross-Chain Transfer Protocol), Paymaster, and Circle's broader stablecoin tooling
* **Sub-Second Block Times**: \~2 second block production tuned for high-throughput payments and order-flow workloads
* **Institutional Backing**: Designed with input from 100+ financial institutions — BlackRock, Visa, HSBC, and others
* **ERC-4337 & EIP-7702 Native**: Account abstraction support built into the network for gas sponsorship, batched transactions, and smart accounts
* **AI Agent Primitives**: Native support for AI agent registration and ERC-8183 job markets, enabling the agentic economy

{% hint style="info" %}
_**TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC SPECIFICATION**_

_GetBlock’s Arc API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for the Ethereum JSON-RPC API is maintained by the Ethereum community and published at_ [_ethereum.org/en/developers/docs/apis/json-rpc/_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_. For Arc-specific protocol details, consensus parameters, the FX engine, and the privacy layer, consult the official documentation at_ [_docs.arc.io_](https://docs.arc.io/)_._
{% endhint %}

{% hint style="warning" %}
**USDC-DENOMINATED GAS — IMPORTANT**

Arc uses USDC as its native gas token, with **6 decimals** (not 18 like most EVM chains). When working with `eth_getBalance`, `eth_gasPrice`, `eth_maxPriorityFeePerGas`, `eth_feeHistory`, and transaction `value` fields, divide raw wei values by 10⁶ — not 10¹⁸ — to obtain USDC. This is the single most common integration mistake when porting an existing EVM dApp to Arc.
{% endhint %}

## Network Information

| Property          | Value                                               |
| ----------------- | --------------------------------------------------- |
| Network Name      | Arc Testnet                                         |
| Stage             | Public testnet (mainnet not yet launched)           |
| Chain ID          | 5042002 (`0x4cef52`)                                |
| Native Currency   | USDC                                                |
| Decimals          | 6 (not 18 — see warning above)                      |
| Block Time        | \~2 seconds                                         |
| Consensus         | Malachite (deterministic sub-second finality)       |
| Smart Contract VM | EVM (Ethereum Virtual Machine)                      |
| EVM Compatible    | Yes (fully compatible)                              |
| Address Format    | Ethereum-style (`0x…`, 20 bytes)                    |
| Block Explorer    | [testnet.arcscan.app](https://testnet.arcscan.app/) |
| Faucet            | [faucet.circle.com](https://faucet.circle.com/)     |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="New York, USA" %}
```bash
https://go.getblock.us/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Singapore, Singapore" %}
```bash
https://go.getblock.asia/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

All Arc JSON-RPC methods are called by sending a `POST` request to the base URL with a standard JSON-RPC 2.0 body. For real-time subscriptions (new blocks, finalized heads, logs) use the WebSocket scheme: `wss://go.getblock.io/<ACCESS-TOKEN>/`.

## Supported Networks

| Network | JSON-RPC | WSS | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | -------- | --- | ------------------ | ------------- | -------------------- |
| Testnet | ✅        | ✅   | ✅                  | ✅             | ✅                    |

Arc mainnet has not yet launched. This documentation targets the **public testnet** (Chain ID `5042002`), which is the current stable development environment.

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
mkdir arc-api-quickstart
cd arc-api-quickstart
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
    "result": "0x4cef52"
}
```

The `result` field is the Arc testnet chain ID (`0x4cef52` = 5042002 in decimal). This confirms you are connected to the correct network.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% stepper %}
{% step %}
#### Setup the project directory

```bash
mkdir arc-api-quickstart
cd arc-api-quickstart
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

Arc exposes the full standard Ethereum JSON-RPC method set. All methods are POST-only and called against the base URL above.

### Block & Chain Information

| Method                                | Description                                                   |
| ------------------------------------- | ------------------------------------------------------------- |
| eth\_blockNumber                      | Returns the current latest block number                       |
| eth\_chainId                          | Returns the chain ID (`0x4cef52` = 5042002 for Arc testnet)   |
| eth\_getBlockByNumber                 | Returns block information by block number                     |
| eth\_getBlockByHash                   | Returns block information by block hash                       |
| eth\_getBlockTransactionCountByNumber | Returns the number of transactions in a block by block number |
| eth\_getBlockTransactionCountByHash   | Returns the number of transactions in a block by block hash   |
| eth\_getBlockReceipts                 | Returns all transaction receipts for a given block            |
| eth\_syncing                          | Returns sync status or `false` if node is fully synced        |

### Account & State

| Method                   | Description                                                       |
| ------------------------ | ----------------------------------------------------------------- |
| eth\_getBalance          | Returns the USDC balance of an account (in 6-decimal base units)  |
| eth\_getCode             | Returns the contract bytecode at a given address                  |
| eth\_getStorageAt        | Returns the value at a specific storage slot                      |
| eth\_getProof            | Returns the Merkle proof for an account and optional storage keys |
| eth\_getTransactionCount | Returns the transaction count (nonce) for an account              |

### Transactions

| Method                                   | Description                                                               |
| ---------------------------------------- | ------------------------------------------------------------------------- |
| eth\_sendRawTransaction                  | Broadcasts a signed transaction                                           |
| eth\_getTransactionByHash                | Returns a transaction by its hash                                         |
| eth\_getTransactionByBlockHashAndIndex   | Returns a transaction by block hash and index                             |
| eth\_getTransactionByBlockNumberAndIndex | Returns a transaction by block number and index                           |
| eth\_getTransactionReceipt               | Returns the receipt for a transaction by hash                             |
| eth\_call                                | Executes a read-only call against contract state                          |
| eth\_estimateGas                         | Estimates gas required for a transaction (denominated in USDC base units) |

### Gas & Fee Market

| Method                    | Description                                                                   |
| ------------------------- | ----------------------------------------------------------------------------- |
| eth\_gasPrice             | Returns the current gas price in USDC base units                              |
| eth\_maxPriorityFeePerGas | Returns the suggested max priority fee per gas (EIP-1559, in USDC base units) |
| eth\_feeHistory           | Returns historical base fees and priority fees                                |

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
| net\_version        | Returns the network ID (`5042002` for Arc testnet)                 |
| net\_listening      | Returns `true` if the client is actively listening for connections |
| net\_peerCount      | Returns the number of peers connected to the node                  |
| web3\_clientVersion | Returns the client software version                                |
| web3\_sha3          | Returns the Keccak-256 hash of the given data                      |

## Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Official Arc Documentation](https://docs.arc.io/)
* [Arc Homepage](https://arc.io/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [arc-node on GitHub (Circle)](https://github.com/circlefin/arc-node)
* [Arc Testnet Explorer (arcscan)](https://testnet.arcscan.app/)
* [Circle USDC Faucet](https://faucet.circle.com/)
* [Arc on Chainlist](https://chainlist.org/chain/5042002)
* [Ethers.js Documentation](https://docs.ethers.org/)
* [Viem Documentation](https://viem.sh/)
* [Circle CCTP (Cross-Chain Transfer Protocol)](https://www.circle.com/cross-chain-transfer-protocol)
