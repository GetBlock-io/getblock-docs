---
description: >-
  Binance Smart Chain API Reference for fast and low-cost interaction with BSC
  nodes, enabling scalable decentralized applications and seamless smart
  contract execution.
---

# BNB Smart Chain (BSC)

**GetBlock provides RPC endpoints that implement the Ethereum JSON-RPC API standard**, which is also adopted by BNB Smart Chain (BSC). This means that all the core methods for interacting with BSC nodes follow this standard. Due to its compatibility with Ethereum, developers can use familiar methods to work with EVM-based networks such as BNB Chain, Polygon, Arbitrum, and others.

#### Overview of BNB Smart Chain Methods

BNB Smart Chain offers a comprehensive suite of methods that allow developers to efficiently interact with its blockchain infrastructure. This overview organizes the methods into key functional categories for easier understanding and implementation.\
\
What you can expect to find in this documentation

* Compatibility with the BNB Smart Chain mainnet and testnet
* Purpose, functionality, and use cases of each method
* Required input parameters
* Sample requests and responses
* Code examples in multiple programming languages (Python, JavaScript)

### What you can do with this API:

* Query real-time blockchain data
* Manage accounts and balances
* Interact with smart contracts
* Submit and track transactions
* Monitor network activity in real time

To effectively use the BNB Smart Chain JSON-RPC methods, you’ll need a tool to send HTTP requests. One such tool is **Axios**, a lightweight and widely used HTTP client.

***

### Quickstart with Axios

**Axios** is a powerful HTTP client that simplifies working with APIs, including BNB Smart Chain’s JSON-RPC interface. Using Axios, you can easily perform transactions, retrieve blockchain data, and more. Below is a step-by-step guide to get started:

#### 1. Set Up Your Project

Choose a package manager and initialize your project:

```bash
mkdir bsc-api-quickstart
cd bsc-api-quickstart
npm init --yes
```

Or using Yarn:

```bash
mkdir bsc-api-quickstart
cd bsc-api-quickstart
yarn init -y
```

#### 2. Install Axios

Install Axios to make HTTP requests:

```bash
npm install axios
```

Or:

```bash
yarn add axios
```

#### 3. Make Your First Request

Create a file called `index.js` and add the following code:

```js
import axios from "axios";

const url = `https://go.getblock.io/<ACCESS-TOKEN>/`;  // Replace <ACCESS-TOKEN> with your actual API key

const payload = {
  jsonrpc: '2.0',
  id: 1,
  method: 'eth_blockNumber',
  params: []
};

axios.post(url, payload)
  .then(response => {
    console.log('Latest block number on BSC:', parseInt(response.data.result, 16));
  })
  .catch(error => {
    console.error(error);
  });
```

You can also use other networks, such as **BSC Testnet**, by changing the URL accordingly.

#### 4. Run Your Script

Execute your script:

```bash
node index.js
```

You should see the latest block number retrieved from the BNB Smart Chain network.

***

### Quickstart with Python and requests

**requests** is a simple yet powerful Python library for making HTTP requests, perfect for working with BNB Smart Chain’s JSON-RPC interface. Here’s how to get started:

#### 1. Set Up Your Project

Create a project directory and virtual environment:

```bash
mkdir bsc-api-quickstart
cd bsc-api-quickstart
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install requests
```

#### 2. Write Your First Script

Create a file named `main.py` and add the following code:

```python
import requests

# Replace <ACCESS-TOKEN> with your actual API key from GetBlock
url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "eth_blockNumber",
    "params": []
}

headers = {
    "Content-Type": "application/json"
}

try:
    response = requests.post(url, json=payload, headers=headers)
    response.raise_for_status()
    data = response.json()

    latest_block = int(data["result"], 16)
    print(f"Latest block number on BSC: {latest_block}")
except requests.exceptions.RequestException as e:
    print(f"An error occurred: {e}")
except KeyError:
    print("Unexpected response format:", response.text)
```

#### 3. Run Your Script

Execute the script with:

```bash
python main.py
```

The latest BSC block number should appear in your console output.

***

#### **Blockchain Information & Network**

* `eth_blockNumber` – Get the latest block number
* `eth_chainId` – Get the chain ID
* `net_version` – Network version
* `net_listening` – Check if the node is listening
* `net_peerCount` – Number of peers connected
* `web3_clientVersion` – Client version
* `web3_sha3` – Keccak-256 hash
* `eth_syncing` – Sync status
* `txpool_status` – Transaction pool status

***

#### &#x20;**Accounts & Wallets**

* `eth_accounts` – List of accounts
* `eth_coinbase` – Miner address (coinbase)
* `eth_getBalance` – Get balance of an address
* `eth_getTransactionCount` – Get nonce of an address
* `eth_sign` – Sign data
* `eth_sendTransaction` – Send a transaction (from unlocked account)
* `eth_sendRawTransaction` – Send a signed transaction

***

#### &#x20;**Transactions**

* `eth_getTransactionByHash`
* `eth_getTransactionReceipt`
* `eth_getTransactionByBlockHashAndIndex`
* `eth_getTransactionByBlockNumberAndIndex`
* `eth_estimateGas` – Estimate gas for a transaction
* `eth_gasPrice` – Current gas price
* `eth_maxPriorityFeePerGas` – Suggested priority fee
* `eth_call` – Call a contract method (read-only)

***

#### &#x20;**Blocks**

* `eth_getBlockTransactionCountByHash`
* `eth_getBlockTransactionCountByNumber`
* `eth_getUncleCountByBlockHash`
* `eth_getUncleCountByBlockNumber`
* `eth_getUncleByBlockHashAndIndex`
* `eth_getUncleByBlockNumberAndIndex`

***

#### &#x20;**Smart Contracts**

* `eth_getCode` – Get smart contract bytecode
* `eth_getStorageAt` – Read contract storage
* `eth_getProof` – Get Merkle proof for account/storage

***

#### &#x20;**Logs & Filters**

* `eth_newFilter`
* `eth_newBlockFilter`
* `eth_newPendingTransactionFilter`
* `eth_getFilterChanges`
* `eth_getFilterLogs`
* `eth_getLogs`
* `eth_uninstallFilter`
* `eth_unsubscribe`

***

#### &#x20;**Debugging / Tracing (debug\_\*)**

* `debug_traceTransaction`
* `debug_traceCall`
* `debug_traceBlock`
* `debug_traceBlockByHash`
* `debug_traceBlockByNumber`
* `debug_standardTraceBadBlockToFile`
* `debug_standardTraceBlockToFile`
* `debug_accountRange`
* `debug_storageRangeAt`

***

#### Conclusion

The BNB Smart Chain methods mirror the Ethereum JSON-RPC standard, providing developers with a familiar and efficient interface for interacting with the blockchain. From retrieving block and transaction data to executing smart contract calls, these methods support a wide array of use cases. The included quickstart guides with Axios and Python make it easy to begin integrating BSC into your applications. By organizing and clarifying these capabilities, this overview helps developers of all experience levels build faster and more confidently on BNB Smart Chain.
