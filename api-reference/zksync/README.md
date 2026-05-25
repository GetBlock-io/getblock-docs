---
description: >-
  zkSync API Reference for efficient interaction with zkSync nodes, enabling
  scalable, low-cost transactions and fast finality through Layer 1 solutions on
  the Ethereum blockchain.
---

# zkSync

zkSync Era is a Layer 2 ZK rollup for Ethereum, developed by Matter Labs and powered by ZK Stack. It scales Ethereum through validity proofs, called cryptographic SNARKs, that verify the correctness of transaction batches before they settle on L1, delivering low fees and near-instant finality without compromising Ethereum's security guarantees.

The network is EVM-equivalent: Solidity and Vyper contracts compile to zkSync-compatible bytecode via the zksolc and zkvyper compilers, and developers can use existing tooling such as Hardhat, Foundry, ethers.js, viem, and Web3.js with minimal changes.&#x20;

zkSync Era extends the standard Ethereum JSON-RPC interface with a `zks_*` namespace for L2-specific operations — bridge contract addresses, L1 batch lookups, fee parameters, paymaster integration, and Merkle proof generation across its unique single-level binary tree architecture.

### Key Features

* **ZK Rollup Architecture**: Validity proofs (SNARKs) cryptographically guarantee that every L2 state transition is correct before it's accepted on L1 — no fraud-proof challenge window
* **EVM-Equivalent**: Deploy existing Solidity / Vyper contracts with minimal changes; full support for Hardhat, Foundry, ethers.js, viem, Web3.js, and zksync-ethers
* **Native Account Abstraction**: First-class support for smart accounts at the protocol level (not bolted on via EIP-4337); paymasters can sponsor gas or accept payment in any token
* **EraVM**: A custom register-based VM optimized for ZK proving, with an EVM Bytecode Interpreter for full equivalence
* **Hyperchains**: zkSync's ZK Stack lets any team launch their own ZK rollup that interoperates with zkSync Era through a shared bridgehub
* **Paymasters**: Built-in protocol-level support for gas sponsorship and ERC-20 fee payment
* **Sub-Second Block Times**: \~1 second block production for low-latency UX
* **L1 Batches**: Transactions are grouped into batches, then committed, proven, and executed on L1 — explorers and indexers operate at the batch granularity via `zks_*` methods
* **Bridgehub Architecture**: All canonical bridge contracts are coordinated through the bridgehub, exposed via `zks_getBridgehubContract` and `zks_getBridgeContracts`
* **Sepolia Testnet**: Public testnet at chain ID 300 for development and testing

{% hint style="info" %}
**TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC SPECIFICATION**

_GetBlock’s zkSync Era API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The standard Ethereum JSON-RPC base is specified by the Ethereum community at_ [_ethereum.org/developers/docs/apis/json-rpc_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_. The canonical specification for the `zks_*` namespace, `debug_*` traces, and pubsub subscriptions specific to zkSync is published at_ [_docs.zksync.io/zksync-protocol/api_](https://docs.zksync.io/zksync-protocol/api)_. For protocol-level details on EraVM, account abstraction, and the bridgehub, consult the official zkSync documentation._
{% endhint %}

## Network Information

| Property          | Value                                                                                      |
| ----------------- | ------------------------------------------------------------------------------------------ |
| Network Name      | zkSync Era Mainnet                                                                         |
| Mainnet Chain ID  | 324 (`0x144`)                                                                              |
| Testnet           | Sepolia testnet (chain ID 300, `0x12c`)                                                    |
| Native Currency   | Ether (ETH)                                                                                |
| Decimals          | 18                                                                                         |
| Block Time        | \~1 second                                                                                 |
| Consensus         | ZK Rollup (validity proofs settled to Ethereum L1)                                         |
| Settlement Layer  | Ethereum mainnet                                                                           |
| Smart Contract VM | EraVM (with EVM Bytecode Interpreter for full equivalence)                                 |
| EVM Compatible    | Yes (EVM-equivalent — full Solidity / Vyper support)                                       |
| Address Format    | Ethereum-style (`0x…`, 20 bytes)                                                           |
| Block Explorer    | [explorer.zksync.io](https://explorer.zksync.io/)                                          |
| L1 Explorer       | [etherscan.io](https://etherscan.io/) (for L1 batch commit / prove / execute transactions) |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
All zkSync Era JSON-RPC methods are called by sending a `POST` request to the base URL with a standard JSON-RPC 2.0 body. For real-time subscriptions (new blocks, logs) use the WebSocket scheme: `wss://go.getblock.io/<ACCESS-TOKEN>/`.
{% endhint %}

## Supported Networks

| Network         | JSON-RPC | WSS | Frankfurt, Germany |
| --------------- | -------- | --- | ------------------ |
| Mainnet         | ✅        | ✅   | ✅                  |
| Sepolia Testnet | ✅        | ✅   | ✅                  |

## Quickstart

In this section, you will learn how to make your first call with either:

* **Axios** (JavaScript / Node.js)
* **Python** (Requests library)

Before you begin, you must have already installed [`npm`](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [`yarn`](https://classic.yarnpkg.com/lang/en/docs/install) on your local machine (for the Axios example) or Python and pip (for the Python example).

{% tabs %}
{% tab title="JavaScript (Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir zksync-api-quickstart
cd zksync-api-quickstart
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
    "result": "0x144"
}
```

The `result` field `0x144` confirms you are connected to zkSync Era mainnet (chain ID 324).
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% stepper %}
{% step %}
### Setup the project directory

```bash
mkdir zksync-api-quickstart
cd zksync-api-quickstart
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
### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

## Available API Methods

zkSync Era exposes the full Ethereum JSON-RPC method set, plus the `zks_*` namespace for L2-specific operations, plus `debug_*` tracing, plus standard `eth_subscribe` WebSocket subscriptions.

### Ethereum: Blocks & Chain

| Method                                | Description                                                 |
| ------------------------------------- | ----------------------------------------------------------- |
| eth\_blockNumber                      | Returns the current latest L2 block number                  |
| eth\_chainId                          | Returns the chain ID (`0x144` = 324 for zkSync Era mainnet) |
| eth\_getBlockByNumber                 | Returns block information by block number                   |
| eth\_getBlockByHash                   | Returns block information by block hash                     |
| eth\_getBlockTransactionCountByNumber | Number of transactions in a block by block number           |
| eth\_getBlockTransactionCountByHash   | Number of transactions in a block by block hash             |
| eth\_getBlockReceipts                 | All transaction receipts for a given block                  |
| eth\_syncing                          | Sync status or `false` if node is fully synced              |

### Ethereum: Account & State

| Method                   | Description                                          |
| ------------------------ | ---------------------------------------------------- |
| eth\_getBalance          | Returns the ETH balance of an account (in wei)       |
| eth\_getCode             | Returns the contract bytecode at a given address     |
| eth\_getStorageAt        | Returns the value at a specific storage slot         |
| eth\_getTransactionCount | Returns the transaction count (nonce) for an account |

### Ethereum: Transactions

| Method                     | Description                                      |
| -------------------------- | ------------------------------------------------ |
| eth\_sendRawTransaction    | Broadcasts a signed transaction                  |
| eth\_getTransactionByHash  | Returns a transaction by its hash                |
| eth\_getTransactionReceipt | Returns the receipt for a transaction by hash    |
| eth\_call                  | Executes a read-only call against contract state |
| eth\_estimateGas           | Estimates gas required for a transaction         |

### Ethereum: Gas, Fees, & Logs

| Method                    | Description                                               |
| ------------------------- | --------------------------------------------------------- |
| eth\_gasPrice             | Returns the current gas price                             |
| eth\_maxPriorityFeePerGas | Returns the suggested max priority fee per gas (EIP-1559) |
| eth\_feeHistory           | Returns historical base fees and priority fees            |
| eth\_getLogs              | Returns logs matching a given filter                      |

### WebSocket Subscriptions

| Method           | Description                               |
| ---------------- | ----------------------------------------- |
| eth\_subscribe   | Subscribes to events (`newHeads`, `logs`) |
| eth\_unsubscribe | Cancels an existing subscription          |

### Network & Client Info

| Method              | Description                                                           |
| ------------------- | --------------------------------------------------------------------- |
| net\_version        | Returns the network ID (`324` for mainnet, `300` for Sepolia testnet) |
| net\_listening      | Returns `true` if the client is actively listening for connections    |
| net\_peerCount      | Returns the number of peers connected to the node                     |
| web3\_clientVersion | Returns the client software version                                   |
| web3\_sha3          | Returns the Keccak-256 hash of the given data                         |

### zkSync: Bridge & Contracts (`zks_*`)

| Method                    | Description                                                      |
| ------------------------- | ---------------------------------------------------------------- |
| zks\_getBridgehubContract | Returns the L1 bridgehub contract address                        |
| zks\_getBridgeContracts   | Returns canonical bridge contract addresses (L1/L2 ERC-20, WETH) |
| zks\_getMainContract      | Returns the main L1 zkSync diamond contract address              |
| zks\_getTestnetPaymaster  | Returns the testnet paymaster address (testnet only)             |

### zkSync: L1 Batches & Blocks (`zks_*`)

| Method                    | Description                                                           |
| ------------------------- | --------------------------------------------------------------------- |
| zks\_L1BatchNumber        | Returns the current L1 batch number                                   |
| zks\_getL1BatchBlockRange | Returns the L2 block range contained in a given L1 batch              |
| zks\_getL1BatchDetails    | Returns details for a specific L1 batch (commit/prove/execute hashes) |
| zks\_getBlockDetails      | Returns zkSync-specific details for a block (status, gas, batch)      |

### zkSync: Fees, Gas, & Proofs (`zks_*`)

| Method                 | Description                                                           |
| ---------------------- | --------------------------------------------------------------------- |
| zks\_getFeeParams      | Returns current fee parameters (L1 gas price, pubdata price, etc.)    |
| zks\_getL1GasPrice     | Returns the current L1 gas price (used in L1→L2 fee calculations)     |
| zks\_gasPerPubdata     | Returns the scaled gas-per-pubdata limit for the currently open batch |
| zks\_estimateGasL1ToL2 | Estimates gas required for an L1 to L2 transaction                    |
| zks\_getProof          | Returns Merkle proofs for storage values (zkSync's binary tree)       |
| zks\_getL2ToL1LogProof | Returns the proof for an L2→L1 log (used for withdrawals)             |

### zkSync: Transactions & Bytecode (`zks_*`)

| Method                                         | Description                                                                  |
| ---------------------------------------------- | ---------------------------------------------------------------------------- |
| zks\_getTransactionDetails                     | Returns zkSync-specific transaction details (L1 commit/prove/execute hashes) |
| zks\_getBytecodeByHash                         | Returns contract bytecode by its hash                                        |
| unstable\_sendRawTransactionWithDetailedOutput | Broadcast tx and return optimistic storage logs + events                     |

### Debug & Tracing (`debug_*`)

| Method                    | Description                                               |
| ------------------------- | --------------------------------------------------------- |
| debug\_traceTransaction   | Replay and trace a transaction by hash                    |
| debug\_traceCall          | Trace a call at a specific block                          |
| debug\_traceBlockByHash   | Trace all transactions within a block specified by hash   |
| debug\_traceBlockByNumber | Trace all transactions within a block specified by number |

## Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Official zkSync Documentation](https://docs.zksync.io/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [zkSync `zks_*` API Reference](https://docs.zksync.io/zksync-protocol/api/zks-rpc)
* [zkSync Debug API Reference](https://docs.zksync.io/zksync-protocol/api/debug-rpc)
* [zkSync PubSub API Reference](https://docs.zksync.io/zksync-protocol/api/pub-sub-rpc)
* [zkSync Era Explorer](https://explorer.zksync.io/)
* [zksync-ethers (JavaScript SDK)](https://github.com/zksync-sdk/zksync-ethers)
* [zksync2-python (Python SDK)](https://github.com/zksync-sdk/zksync2-python)
* [Ethers.js Documentation](https://docs.ethers.org/)
* [Viem Documentation](https://viem.sh/)
* [Hardhat zkSync plugin](https://docs.zksync.io/build/tooling/hardhat/getting-started)
