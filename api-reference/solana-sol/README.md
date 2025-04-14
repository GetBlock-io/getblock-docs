# Solana (SOL)

GetBlock provides RPC endpoints that implement the Solana JSON-RPC API standard. These methods are core functionalities for interacting with Solana nodes, enabling seamless integration with the Solana blockchain.

### Overview of Solana Network Methods

The Solana network offers a comprehensive suite of methods that enable developers to interact seamlessly with its blockchain infrastructure. This overview provides an in-depth examination of these methods, categorizing them into key functional areas for better understanding and implementation.

What you can expect to find in this documentation:

* Compatibility with the Solana mainnet and testnets
* Purpose, functionality, and use cases of each Solana method
* Required input parameters
* Sample requests and responses
* Code examples in multiple programming languages (Python, JavaScript)

What you can do with this API:

* Query real-time blockchain data
* Manage and monitor accounts and balances
* Interact with smart contracts
* Submit and track transactions
* Monitor network activity in real-time

To effectively use the Solana methods listed above, you'll need a reliable tool to send requests to the network. One such tool is Axios, a lightweight and widely used HTTP client. With Axios, you can easily interact with Solana's JSON-RPC API to execute transactions, retrieve blockchain data, and much more.

### Quickstart with Axios

Axios is a powerful HTTP client that streamlines communication with APIs, including Solana's JSON-RPC interface. By using Axios, developers can effortlessly send requests and process responses, making blockchain integration straightforward and efficient. Below, we provide a detailed guide to help you get started:

1. Set Up Your Project

Choose a package manager and initialize your project:

```bash
mkdir solana-api-quickstart
cd solana-api-quickstart
npm init --yes
```

Or, using Yarn:

```bash
mkdir solana-api-quickstart
cd solana-api-quickstart
yarn init -y
```

2. Install Axios

Install Axios to make API requests:

```bash
npm install axios
```

Or, using Yarn:

```bash
yarn add axios
```

3. Make Your First Request

Set up a new file named `index.js` and insert the code snippet provided below:

```javascript
import axios from "axios";

const url = `https://go.getblock.io/<ACCESS-TOKEN>/`;
const payload = {
  jsonrpc: '2.0',
  id: 1,
  method: 'getBalance',
  params: ['<ACCOUNT-PUBKEY>']
};

axios.post(url, payload)
  .then(response => {
    console.log('Account balance:', response.data.result.value);
  })
  .catch(error => {
    console.error(error);
  });
```

Replace `<ACCESS-TOKEN>` with your actual API key from GetBlock, and `<ACCOUNT-PUBKEY>` with the public key of the account you wish to query.

4. Run Your Script

Execute your script with:

```bash
node index.js
```

You should see the account balance logged to your console.

### Quickstart with Python and requests

requests is a powerful and easy-to-use HTTP library for Python that simplifies API interactions, including Solana’s JSON-RPC API. With requests, developers can send requests, handle responses, and easily integrate blockchain functionality into their projects. Here’s a step-by-step guide to get started:

1. Set Up Your Project

Create a new directory for your project and navigate into it:

```bash
mkdir solana-api-quickstart
cd solana-api-quickstart
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

2. Write Your First Script

Create a new file called `main.py` and insert the following code:

```python
import requests

# Replace <ACCESS-TOKEN> with your actual API key from GetBlock
url = "https://go.getblock.io/<ACCESS-TOKEN>/"

# Create a JSON-RPC payload
payload = {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getBalance",
    "params": ["<ACCOUNT-PUBKEY>"]
}

headers = {
    "Content-Type": "application/json"
}

try:
    # Send the POST request
    response = requests.post(url, json=payload, headers=headers)
    response.raise_for_status()  # Check for HTTP errors
    data = response.json()

    # Print the account balance
    balance = data["result"]["value"]
    print(f"Account balance: {balance}")
except requests.exceptions.RequestException as e:
    print(f"An error occurred: {e}")
except KeyError:
    print("Unexpected response format:", response.text)
```

Replace \<ACCESS-TOKEN> with your actual API key from GetBlock, and \<ACCOUNT-PUBKEY> with the public key of the account you wish to query.

3. Run Your Script

Execute the script with:

```bash
python main.py
```

You should see the account balance retrieved from the Solana network printed to your console.

***

### **Network & Node Info**

* getClusterNodes
* getHealth
* getVersion
* getIdentity
* getGenesisHash

***

### **Block & Slot Info**

* getBlock
* getBlockCommitment
* getBlockHeight
* getBlockProduction
* getBlocks
* getBlocksWithLimit
* getBlockTime
* getFirstAvailableBlock
* getHighestSnapshotSlot
* getSlot
* getSlotLeader
* getSlotLeaders
* minimumLedgerSlot

***

### **Account Management**

* getAccountInfo
* getMultipleAccounts
* getProgramAccounts
* getTokenAccountBalance
* getTokenAccountsByDelegate
* getTokenAccountsByOwner
* getTokenLargestAccounts
* getTokenSupply
* getMinimumBalanceForRentExemption
* requestAirdrop
* getBalance

***

### **Transactions**

* sendTransaction
* simulateTransaction
* getTransaction
* getTransactionCount
* getSignatureStatuses
* getSignaturesForAddress
* getFeeForMessage
* getLatestBlockhash
* isBlockhashValid

***

### **Epoch & Stake**

* getEpochInfo
* getEpochSchedule
* getInflationGovernor
* getInflationRate
* getInflationReward
* getLeaderSchedule
* getStakeMinimumDelegation

***

### **Performance & Supply**

* getRecentPerformanceSamples
* getRecentPrioritizationFees
* getSupply

***

### **Governance / Validators**

* getVoteAccounts

***

### **Subscriptions (WebSocket Only)**

* accountSubscribe
* accountUnsubscribe
* blockSubscribe
* blockUnsubscribe
* logsSubscribe
* logsUnsubscribe
* programSubscribe
* programUnsubscribe
* rootSubscribe
* rootUnsubscribe
* signatureSubscribe
* signatureUnsubscribe
* slotSubscribe
* slotUnsubscribe
* slotsUpdatesSubscribe
* slotsUpdatesUnsubscribe
* voteSubscribe
* voteUnsubscr

***

### Conclusion

Solana’s JSON-RPC API offers a fast and efficient way to interact with the network—whether you're managing accounts, sending transactions, or tracking real-time data.

Grouped by functionality, the methods cover everything needed to build scalable Web3 applications with ease. With HTTP and WebSocket support, and tools like Axios and requests, integration is simple for developers of all levels.

Start building today and explore Solana’s full capabilities.
