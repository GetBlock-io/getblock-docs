---
description: >-
  Avalanche Network API Reference for efficient interaction with AVAX nodes,
  enabling fast, low-cost Layer 2 scaling solutions for Ethereum with high
  throughput and secure smart contracts.
---

# Avalanche (AVAX)

Avalanche is a high-throughput, low-latency Proof-of-Stake blockchain platform launched in September 2020 by Ava Labs, founded by Emin Gün Sirer, Kevin Sekniqi, and Maofan "Ted" Yin. Its defining innovation is a novel family of consensus protocols:

1. &#x20;Avalanche consensus (DAG-based) and&#x20;
2. Snowman consensus (linear chains) achieves sub-second finality through repeated random subsampling rather than traditional PBFT or longest-chain rules.&#x20;

Avalanche's most distinctive architectural feature is its **three-chain Primary Network**:&#x20;

1. The Contract Chain (C-Chain) for EVM-compatible smart contracts
2. the Platform Chain (P-Chain) for validators, staking, and subnet metadata,
3. The Exchange Chain (X-Chain) for native asset transfers.&#x20;

Each chain has its own virtual machine, its own RPC namespace, and its own URL path under the GetBlock base URL. The network also pioneered the concept of Subnets (now called L1S), application-specific blockchains with custom validators and tokenomics.

### Key Features

* **Three-Chain Primary Network**: C-Chain (EVM, smart contracts), P-Chain (`platform.*` for staking and subnets), X-Chain (`avm.*` for native assets) — each with its own RPC path
* **Sub-Second Finality**: Repeated random subsampling delivers irreversibility in under 1 second, with no probabilistic confirmation windows
* **EVM Compatibility on C-Chain**: Full Ethereum JSON-RPC support; deploy Solidity / Vyper contracts with Hardhat, Foundry, ethers.js, viem, or web3.py without modification
* **Avalanche `eth_*` Extensions on C-Chain**: `eth_baseFee`, `eth_getChainConfig`, `eth_suggestPriceOptions`, `eth_getBadBlocks`, and the `newAcceptedTransactions` WebSocket subscription
* **Atomic Cross-Chain Transfers**: `avax.export` / `avax.import` enable atomic AVAX movement between C, P, and X chains
* **Snowman Consensus**: Used by C-Chain and P-Chain — linear, BFT-style finality optimized for smart contract execution
* **Avalanche Consensus**: Used by X-Chain — DAG-based, optimized for parallel asset transfers
* **Subnets (L1s)**: Custom application-specific blockchains with their own validator sets, virtual machines, and token economics
* **Native AVAX**: Network's native token (18 decimals on C-Chain, 9 decimals as nAVAX on P-Chain / X-Chain — see warning below)
* **Production-Grade Scalability**: 4,500+ TPS theoretical throughput on the Primary Network, with subnets adding horizontal scale

{% hint style="info" %}
**TECHNICAL DISCLAIMER: AUTHORITATIVE AVALANCHE RPC SPECIFICATION**

_GetBlock’s Avalanche API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical specification for Avalanche's RPC interfaces is maintained by Ava Labs and published at_ [_build.avax.network/docs/rpcs_](https://build.avax.network/docs/rpcs/)_. The C-Chain follows the standard Ethereum JSON-RPC specification at_ [_ethereum.org/developers/docs/apis/json-rpc_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_, plus Avalanche-specific extensions documented separately. For P-Chain and X-Chain protocols, consensus parameters, and subnet architecture, consult the_ [_official Avalanche documentation_](https://build.avax.network/docs)_._
{% endhint %}

{% hint style="warning" %}
**AVAX DECIMALS DIFFER BETWEEN CHAINS — IMPORTANT**

AVAX is represented with different decimal precision depending on which chain you are interacting with:

* **C-Chain**: 18 decimals (1 AVAX = 10¹⁸ wei), matching Ethereum's convention. Use `parseEther` / `formatEther`.
* **P-Chain and X-Chain**: 9 decimals — denominated in **nAVAX** (nano-AVAX). 1 AVAX = 10⁹ nAVAX.

When transferring AVAX across chains via `avax.export` / `avax.import` or `platform.exportAVAX` / `platform.importAVAX`, the amount field uses the **destination chain's decimal convention**. A common integration bug is sending 18-decimal wei amounts to the P-Chain (which interprets them as nAVAX).
{% endhint %}

## Network Information

| Property                   | Value                                                                                               |
| -------------------------- | --------------------------------------------------------------------------------------------------- |
| Network Name               | Avalanche Mainnet (Primary Network)                                                                 |
| C-Chain ID                 | 43114 (`0xa86a`)                                                                                    |
| Fuji Testnet C-Chain ID    | 43113 (`0xa869`)                                                                                    |
| Native Currency            | AVAX                                                                                                |
| C-Chain Decimals           | 18 (wei)                                                                                            |
| P-Chain / X-Chain Decimals | 9 (nAVAX)                                                                                           |
| Finality                   | Sub-second (Avalanche / Snowman consensus)                                                          |
| Smart Contract VM          | EVM on C-Chain; AvalancheVM on X-Chain; Platform VM on P-Chain                                      |
| EVM Compatible             | Yes (C-Chain only)                                                                                  |
| Address Format             | C-Chain: Ethereum-style `0x…`; P-Chain / X-Chain: Bech32 `P-avax1…` / `X-avax1…`                    |
| Block Explorer             | [snowtrace.io](https://snowtrace.io/) (C-Chain), [avascan.info](https://avascan.info/) (all chains) |

## Base URL

GetBlock exposes all three Avalanche chains under a single regional base URL. The chain is selected by the URL path:

| Chain   | Path suffix     | Use for                                                |
| ------- | --------------- | ------------------------------------------------------ |
| C-Chain | `/ext/bc/C/rpc` | EVM smart contracts, `eth_*`, `avax.*` cross-chain ops |
| P-Chain | `/ext/bc/P`     | `platform.*` — validators, staking, subnets            |
| X-Chain | `/ext/bc/X`     | `avm.*` — native asset transfers                       |

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc
https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P
https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/X
```
{% endtab %}
{% endtabs %}

All methods are called by sending a `POST` request to the corresponding chain URL with a JSON-RPC 2.0 body. For real-time subscriptions on the C-Chain, use the WebSocket scheme: `wss://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/ws`. **WebSocket is only available on the C-Chain** — P-Chain and X-Chain are HTTP only.

## Supported Networks

| Network      | C-Chain JSON-RPC | C-Chain WSS | P-Chain | X-Chain | Frankfurt |
| ------------ | ---------------- | ----------- | ------- | ------- | --------- |
| Mainnet      | ✅                | ✅           | ✅       | ✅       | ✅         |
| Fuji Testnet | ✅                | ✅           | ✅       | ✅       | ✅         |

## Quickstart

In this section, you will learn how to make your first call against the **C-Chain** (the most common starting point) with either:

* **Axios** (JavaScript / Node.js)
* **Python** (Requests library)

Before you begin, you must have already installed [`npm`](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [`yarn`](https://classic.yarnpkg.com/lang/en/docs/install) on your local machine (for the Axios example) or Python and pip (for the Python example).

{% tabs %}
{% tab title="JavaScript (Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir avalanche-api-quickstart
cd avalanche-api-quickstart
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
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc',
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
### Run the script

```bash
node index.js
```

Expected output:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0xa86a"
}
```

`0xa86a` (43114 in decimal) confirms you are connected to Avalanche C-Chain mainnet.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% stepper %}
{% step %}
### Setup the project directory

```bash
mkdir avalanche-api-quickstart
cd avalanche-api-quickstart
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

{% code title="main.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc"

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
### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

## Available API Methods

Avalanche exposes three RPC surfaces. C-Chain is the most heavily used (EVM smart contracts). P-Chain and X-Chain are documented here with their core methods; for the complete `platform.*` and `avm.*` surface, see [build.avax.network/docs/rpcs](https://build.avax.network/docs/rpcs/).

### C-Chain: Ethereum-Compatible (`eth_*`)

| Method                     | Description                                                 |
| -------------------------- | ----------------------------------------------------------- |
| eth\_blockNumber           | Returns the current latest block number                     |
| eth\_chainId               | Returns the chain ID (`0xa86a` = 43114 for C-Chain mainnet) |
| eth\_getBlockByNumber      | Returns block information by block number                   |
| eth\_getBlockByHash        | Returns block information by block hash                     |
| eth\_getBlockReceipts      | Returns all transaction receipts for a given block          |
| eth\_syncing               | Returns sync status or `false` if node is fully synced      |
| eth\_getBalance            | Returns the AVAX balance of an account (in wei)             |
| eth\_getCode               | Returns the contract bytecode at a given address            |
| eth\_getStorageAt          | Returns the value at a specific storage slot                |
| eth\_getTransactionCount   | Returns the transaction count (nonce) for an account        |
| eth\_sendRawTransaction    | Broadcasts a signed transaction                             |
| eth\_getTransactionByHash  | Returns a transaction by its hash                           |
| eth\_getTransactionReceipt | Returns the receipt for a transaction by hash               |
| eth\_call                  | Executes a read-only call against contract state            |
| eth\_estimateGas           | Estimates gas required for a transaction                    |
| eth\_gasPrice              | Returns the current gas price                               |
| eth\_maxPriorityFeePerGas  | Returns the suggested max priority fee per gas (EIP-1559)   |
| eth\_feeHistory            | Returns historical base fees and priority fees              |
| eth\_getLogs               | Returns logs matching a given filter                        |

### C-Chain: Avalanche `eth_*` Extensions

| Method                   | Description                                                  |
| ------------------------ | ------------------------------------------------------------ |
| eth\_baseFee             | Returns the base fee for the next block (Avalanche-specific) |
| eth\_getChainConfig      | Returns the C-Chain configuration                            |
| eth\_suggestPriceOptions | Returns suggested slow / normal / fast gas price options     |

### C-Chain: WebSocket Subscriptions

| Method           | Description                                                                                    |
| ---------------- | ---------------------------------------------------------------------------------------------- |
| eth\_subscribe   | Subscribes to events (`newHeads`, `logs`, `newPendingTransactions`, `newAcceptedTransactions`) |
| eth\_unsubscribe | Cancels an existing subscription                                                               |

### C-Chain: Network & Client Info

| Method              | Description                                                        |
| ------------------- | ------------------------------------------------------------------ |
| net\_version        | Returns the network ID                                             |
| net\_listening      | Returns `true` if the client is actively listening for connections |
| net\_peerCount      | Returns the number of peers connected to the node                  |
| web3\_clientVersion | Returns the client software version                                |
| web3\_sha3          | Returns the Keccak-256 hash of the given data                      |

### P-Chain (`platform.*`)

| Method                        | Description                                               |
| ----------------------------- | --------------------------------------------------------- |
| platform.getHeight            | Returns the current P-Chain block height                  |
| platform.getCurrentValidators | Returns the list of current validators for a given subnet |
| platform.getMinStake          | Returns the minimum amount of AVAX required to stake      |
| platform.getStakingAssetID    | Returns the asset ID used for staking on a subnet         |
| platform.getCurrentSupply     | Returns the current AVAX supply on the P-Chain            |
| platform.getTotalStake        | Returns the total amount of AVAX staked on a subnet       |

### X-Chain (`avm.*`)

| Method                  | Description                                                          |
| ----------------------- | -------------------------------------------------------------------- |
| avm.getAssetDescription | Returns description (name, symbol, decimals) for an asset            |
| avm.getTx               | Returns a transaction by ID                                          |
| avm.getTxStatus         | Returns the status of a transaction (`Accepted`, `Processing`, etc.) |
| avm.getUTXOs            | Returns the UTXOs that reference a given address                     |
| avm.issueTx             | Sends a signed transaction to the X-Chain                            |

## Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Official Avalanche Builder Docs](https://build.avax.network/docs)
* [Avalanche RPC Reference](https://build.avax.network/docs/rpcs)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [AvalancheJS SDK (P-Chain / X-Chain)](https://github.com/ava-labs/avalanchejs)
* [Snowtrace (C-Chain Explorer)](https://snowtrace.io/)
* [Avascan (Multi-Chain Explorer)](https://avascan.info/)
* [Avalanche on Chainlist](https://chainlist.org/chain/43114)
* [Ethers.js Documentation](https://docs.ethers.org/)
* [Viem Documentation](https://viem.sh/)
