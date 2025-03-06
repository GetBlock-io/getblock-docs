---
description: >-
  This Ethereum API Reference provides everything you need to interact with
  Ethereum nodes using GetBlock’s infrastructure.
---

# Ethereum (ETH)

GetBlock provides RPC endpoints that implement the Ethereum JSON-RPC API standard. These methods are core functionalities for interacting with Ethereum nodes. Many Ethereum-compatible blockchains (EVM chains) – such as BNB Chain, Polygon, Linea, Base, Optimism, and Arbitrum – also adopt this standard, making it applicable across various ecosystems.

**Overview of Ethereum Network Methods**

The Ethereum network offers a comprehensive suite of methods that enable developers to interact seamlessly with its blockchain infrastructure. This overview provides an in-depth examination of these methods, categorizing them into key functional areas for better understanding and implementation.

### What you can expect to find in this documentation

* Compatibility with the Ethereum mainnet and testnets
* Purpose, functionality, and use cases of each Ethereum method
* Required input parameters
* Sample requests and responses
* Code examples in multiple programming languages (Python, JavaScript)

### What you can do with this API: <a href="#what-you-can-do-with-this-api" id="what-you-can-do-with-this-api"></a>

* Query real-time blockchain data
* Manage and monitor accounts and balances.
* Interact with smart contracts
* Submit and track transactions
* Monitor network activity in real-time&#x20;

***

To effectively use the Ethereum methods listed above, you'll need a reliable tool to send requests to the network. One such tool is Axios, a lightweight and widely used HTTP client. With Axios, you can easily interact with Ethereum's JSON-RPC API to execute transactions, retrieve blockchain data, and much more.

#### Quickstart with Axios

Axios is a powerful HTTP client that streamlines communication with APIs, including Ethereum's JSON-RPC interface. By using Axios, developers can effortlessly send requests and process responses, making blockchain integration straightforward and efficient. Below, we provide a detailed guide to help you get started:

Developers new to Ethereum can efficiently interact with the Ethereum API by leveraging Axios, a well-regarded HTTP client. It simplifies API integration, making it user-friendly and effective. Here’s how you can get started:

**1. Set Up Your Project**

Choose a package manager and initialize your project:

```
mkdir ethereum-api-quickstart
cd ethereum-api-quickstart
npm init --yes
```

Or, using Yarn:

```
mkdir ethereum-api-quickstart
cd ethereum-api-quickstart
yarn init -y
```

**2. Install Axios**

Install Axios to make API requests:

```
npm install axios
```

Or, using Yarn:

```
yarn add axios
```

**3. Make Your First Request**

Set up a new file named index.js and insert the code snippet provided below:

```javascript
import axios from "axios";


const url = `https://go.getblock.io/<ACCESS-TOKEN>/`;


const payload = {
  jsonrpc: '2.0',
  id: 1,
  method: 'eth_blockNumber',
  params: []
};


axios.post(url, payload)
  .then(response => {
    console.log('Latest block number', parseInt(response.data.result, 16));
  })
  .catch(error => {
    console.error(error);
  });
```

Replace \<ACCESS-TOKEN> with your actual API key from the GetBlock. You can also use other networks by replacing the URL with those of Sepolia or Holesky testnets.

**4. Run Your Script**

Execute your script with:

node index.js

You should see the latest block number logged to your console.\


#### Quickstart with Python and requests

requests is a powerful and easy-to-use HTTP library for Python that simplifies API interactions, including Ethereum’s JSON-RPC API. With requests, developers can send requests, handle responses, and easily integrate blockchain functionality into their projects. Here’s a step-by-step guide to get started:

**1. Set Up Your Project**

Create a new directory for your project and navigate into it:

```bash
mkdir ethereum-api-quickstart
cd ethereum-api-quickstart
```

Set up a virtual environment to isolate dependencies:

```bash
python -m venv venv
source venv/bin/activate  # On Windows, use venv\Scripts\activate
```

Install the requests library:

```bash
 pip install requests
```

**2. Write Your First Script**

Create a new file called main.py and insert the following code:

```python
python import requests

# Replace <ACCESS-TOKEN> with your actual API key from GetBlock
url = "https://go.getblock.io/<ACCESS-TOKEN>/"

# Create a JSON-RPC payload
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
    # Send the POST request
    response = requests.post(url, json=payload, headers=headers)
    response.raise_for_status()  # Check for HTTP errors
    data = response.json()

    # Convert the result to a decimal number and print it
    latest_block = int(data["result"], 16)
    print(f"Latest block number: {latest_block}")
except requests.exceptions.RequestException as e:
    print(f"An error occurred: {e}")
except KeyError:
    print("Unexpected response format:", response.text)
```

**3. Run Your Script**

Execute the script with:

```bash
 python main.py
```

You should see the latest block number retrieved from the Ethereum network printed to your console.

***

\
**1. Account and Balance Management**

These methods allow users to retrieve information about Ethereum accounts and their balances.

* eth\_getBalance: Allows developers to query the Ether balance of any account at a precise blockchain height for better transaction tracking and financial auditing.
  * Parameters: Address, Block Identifier
  * Response: Balance in Wei
* eth\_accounts: Lists all accounts managed by the connected Ethereum node.
  * Response: Array of account addresses\


**2. Block and Transaction Retrieval**

Key methods to fetch block and transaction data from the blockchain.

* eth\_blockNumber: Retrieves the latest block number.
  * Response: Block number
* eth\_getBlockByNumber: Fetches a block by its number.
  * Parameters: Block Number, Boolean (for full transaction objects)
  * Response: Block data
* eth\_getBlockByHash: Retrieves a block using its hash.
  * Parameters: Block Hash, Boolean (for full transaction objects)
  * Response: Block data
* eth\_getTransactionByHash: Fetches details of a specific transaction.
  * Parameters: Transaction Hash
  * Response: Transaction object
* eth\_getTransactionReceipt: Provides the receipt of a processed transaction.
  * Parameters: Transaction Hash
  * Response: Receipt object
* eth\_getBlockReceipts: Retrieves all transaction receipts for a specific block.
  * Parameters: Block Identifier (hash or number)
  * Response: Array of transaction receipts
* eth\_getBlockTransactionCountByHash: Retrieves the number of transactions in a block identified by its hash.
  * Parameters: Block Hash
  * Response: Integer
* eth\_getBlockTransactionCountByNumber: Retrieves the number of transactions in a block identified by its number.
  * Parameters: Block Number
  * Response: Integer
* eth\_getTransactionByBlockHashAndIndex: Retrieves a transaction by block hash and index.
  * Parameters: Block Hash, Index
  * Response: Transaction object
* eth\_getTransactionByBlockNumberAndIndex: Retrieves a transaction by block number and index.
  * Parameters: Block Number, Index
  * Response: Transaction object

***

**3. Smart Contract Interaction**

Methods to interact with deployed smart contracts on the Ethereum network.

* eth\_call: Executes a read-only call to a smart contract.
  * Parameters: Transaction object, Block Identifier
  * Response: Returned data from the contract
* eth\_estimateGas: Estimates the gas required to execute a transaction.
  * Parameters: Transaction object
  * Response: Estimated gas value
* eth\_getCode: Retrieves the bytecode of a smart contract.
  * Parameters: Address, Block Identifier
  * Response: Contract bytecode
* eth\_getStorageAt: Retrieves the value of a specific storage slot at an address.
  * Parameters: Address, Storage Slot, Block Identifier
  * Response: Data
* eth\_getProof: Retrieves the Merkle proof for a storage slot.
  * Parameters: Address, Storage Slots, Block Identifier
  * Response: Proof object

***

**4. Debug and Trace Utilities**

Advanced methods for developers requiring detailed blockchain insights.

* debug\_accountRange: Fetches a specified range of accounts based on a particular block height for detailed analysis.
  * Parameters: Block Identifier, Start Account, Limit
  * Response: List of accounts
* debug\_batchSendRawTransaction: Sends a batch of raw transactions to the network.
  * Parameters: Array of raw transactions
  * Response: Array of transaction hashes
* debug\_getBadBlocks: Lists blocks considered invalid by the node.
  * Response: Array of block details
* debug\_storageRangeAt: Retrieves a range of storage keys from a specific address.
  * Parameters: Block Identifier, Address, Start Key, Limit
  * Response: Storage range details
* debug\_traceBlock: Traces all transactions in a block by number.
  * Parameters: Block Number
  * Response: Array of traces
* debug\_traceBlockByHash: Traces all transactions in a block by hash.
  * Parameters: Block Hash
  * Response: Array of traces
* debug\_traceBlockByNumber: Traces all transactions in a block by number.
  * Parameters: Block Number
  * Response: Array of traces
* debug\_traceCall: Simulates a contract call for debugging.
  * Parameters: Transaction object, Block Identifier, Options
  * Response: Trace details
* debug\_traceTransaction: Provides an execution trace of a transaction.
  * Parameters: Transaction Hash
  * Response: Trace details

***

**5. Mining and Node Management**

Methods to manage and monitor the Ethereum node itself.

* eth\_mining: Indicates if the node is currently mining.
  * Response: Boolean
* eth\_hashrate: Provides the current hash rate of the node.
  * Response: Hash rate in hashes per second
* eth\_syncing: Provides information about the node's syncing status, indicating whether it is fully synchronized with the network.
  * Response: Syncing object or false
* eth\_submitWork: Submits a proof-of-work solution.
  * Parameters: Nonce, PowHash, MixDigest
  * Response: Boolean
* eth\_submitHashrate: Submits the hash rate of a mining node.
  * Parameters: Hashrate, ID
  * Response: Boolean
* eth\_getWork: Provides the data a miner needs to produce the proof-of-work.
  * Response: Work data
* eth\_coinbase: Retrieves the current coinbase address (the address to which mining rewards are directed).
  * Response: Address

***

**6. Event and Log Management**

Facilitate tracking of specific events on the blockchain.

* eth\_getLogs: Retrieves logs matching filter criteria.
  * Parameters: Filter object (addresses, topics, block range)
  * Response: Array of logs
* eth\_newFilter: Creates a new filter object for events.
  * Parameters: Filter object
  * Response: Filter ID
* eth\_uninstallFilter: Removes a filter object.
  * Parameters: Filter ID
  * Response: Boolean
* eth\_newBlockFilter: Creates a filter for new block notifications.
  * Response: Filter ID
* eth\_newPendingTransactionFilter: Creates a filter for pending transaction notifications.
  * Response: Filter ID
* eth\_getFilterChanges: Retrieves changes for a given filter.
  * Parameters: Filter ID
  * Response: Array of changes
* eth\_getFilterLogs: Retrieves all logs for a filter.
  * Parameters: Filter ID
  * Response: Array of logs

***

**7. Miscellaneous Utility Methods**

These methods provide various utilities to aid Ethereum developers.

* eth\_gasPrice: Retrieves the current gas price in the network.
  * Response: Gas price in Wei
* eth\_chainId: Returns the ID of the current blockchain.
  * Response: Chain ID
* eth\_maxPriorityFeePerGas: Retrieves the maximum priority fee per gas.
  * Response: Gas fee in Wei
* eth\_feeHistory: Provides historical data on gas fees.
  * Parameters: Block Count, Newest Block, Reward Percentiles
  * Response: Fee history object
* eth\_sendRawTransaction: Sends a raw transaction to the network.
  * Parameters: Signed transaction data
  * Response: Transaction hash
* web3\_clientVersion: Provides the client software version.
  * Response: String
* web3\_sha3: Returns the Keccak-256 hash of input data.
  * Parameters: Data (hex string)
  * Response: Hash (hex string)
* rpc\_modules: Lists available RPC modules.
  * Response: Object of module names and versions
* net\_listening: Indicates if the node is accepting connections.
  * Response: Boolean
* net\_peerCount: Returns the number of connected peers.
  * Response: Integer
* net\_version: Provides the network ID.
  * Response: String
* eth\_subscribe: Subscribes to a particular event stream, such as new blocks or logs.
  * Parameters: Subscription type, Options
  * Response: Subscription ID
* eth\_unsubscribe: Unsubscribes from a previously created subscription.
  * Parameters: Subscription ID
  * Response: Boolean

***

#### Conclusion

The Ethereum network methods are versatile and cater to diverse use cases, from simple account balance queries to advanced transaction tracing. The added quickstart guide demonstrates how to leverage Axios for making efficient API requests, enabling developers to start building on Ethereum seamlessly. By categorizing and simplifying these concepts, this overview aims to enhance productivity for both new and experienced Ethereum developers.
