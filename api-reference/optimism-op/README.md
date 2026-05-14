---
description: >-
  Optimism Network API Reference for efficient interaction with OP nodes,
  enabling scalable, low-cost transactions and fast finality through Layer 2
  solutions on the Ethereum blockchain.
---

# Optimism (OP)

Optimism is a Layer 2 scaling solution for Ethereum that uses Optimistic Rollup technology to achieve high throughput and low transaction costs while inheriting Ethereum's security. Developed by the Optimism Collective, it enables near-instant transactions by executing them off-chain and posting compressed transaction data to Ethereum mainnet. The network is EVM-equivalent, meaning existing Ethereum smart contracts and tools work seamlessly without modification.

## Key Features

* **EVM Equivalence**: Full compatibility with Ethereum Virtual Machine, allowing direct deployment of existing Solidity contracts
* **Optimistic Rollups**: Transactions are assumed valid by default, with a 7-day challenge period for fraud proofs
* **Low Transaction Fees**: Fees typically 10-100x lower than Ethereum mainnet
* **Fast Transactions**: Near-instant transaction confirmations (\~2 second block time)
* **Ethereum Security**: Inherits security from Ethereum L1 through data availability and fraud proofs
* **OP Token**: Native governance token for the Optimism Collective
* **Superchain Vision**: Part of the broader Superchain ecosystem with shared security and interoperability
* **Bedrock Upgrade**: Latest architecture providing improved performance and EVM equivalence
* **Developer-Friendly**: Compatible with MetaMask, Hardhat, Foundry, and all standard Ethereum tools

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for Ethereum Virtual Machine (EVM) JSON-RPC methods is solely maintained and published through the official Ethereum Foundation documentation portal at_ [_ethereum.org_](http://ethereum.org/)_. For Optimism-specific methods, refer to the official Optimism documentation at_ [_docs.optimism.io_](https://docs.optimism.io/)_._
{% endhint %}

## Network Information

| Property        | Value                |
| --------------- | -------------------- |
| Network Name    | Optimism Mainnet     |
| Chain ID        | 10                   |
| Native Currency | ETH                  |
| Decimals        | 18                   |
| Block Time      | \~2 seconds          |
| Consensus       | Optimistic Rollup    |
| EVM Compatible  | Yes (EVM Equivalent) |
| L1 Settlement   | Ethereum Mainnet     |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network         | Chain ID | JSON-RPC | WSS | Frankfurt, Germany | GraphQL |
| --------------- | -------: | -------- | --- | ------------------ | ------- |
| Mainnet         |       10 | ✅        | ✅   | ✅                  | ✅       |
| Sepolia Testnet | 11155420 | ✅        | ✅   | ✅                  | ❌       |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
#### Setup project

Create and initialize a new project:

```bash
mkdir optimism-api-quickstart
cd optimism-api-quickstart
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
import axios from 'axios'
let data = JSON.stringify({
  jsonrpc: "2.0",
  method: "eth_blockNumber",
  params: [],
  id: "getblock.io",
});

let config = {
  method: "post",
  maxBodyLength: Infinity,
  url: "https://go.getblock.us/<ACCESS_TOKEN>",
  headers: {
    "Content-Type": "application/json",
  },
  data: data,
};

axios
  .request(config)
  .then((response) => {
    console.log(JSON.stringify(response.data));
  })
  .catch((error) => {
    console.log(error);
  });

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
    "result": "0x907de6b"
}
```
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python(Request)" %}
{% stepper %}
{% step %}
#### Set up the project directory

```bash
mkdir optimism-api-quickstart
cd optimism-api-quickstart
```
{% endstep %}

{% step %}
#### Create and activate a virtual environment

```bash
python -m venv venv
source venv/bin/activate

# On Windows, 
use venv\Scripts\activate
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

### Account/State Methods

| Method            | Description                                    |
| ----------------- | ---------------------------------------------- |
| eth\_accounts     | Returns addresses owned by client              |
| eth\_getBalance   | Returns the ETH balance of an address          |
| eth\_getStorageAt | Returns the value at a storage position        |
| eth\_getCode      | Returns the bytecode at an address             |
| eth\_getProof     | Returns account and storage Merkle proofs      |
| eth\_call         | Executes a call without creating a transaction |

### Block Methods

| Method                                | Description                                    |
| ------------------------------------- | ---------------------------------------------- |
| eth\_blockNumber                      | Returns the current block number               |
| eth\_getBlockByHash                   | Returns block information by hash              |
| eth\_getBlockByNumber                 | Returns block information by number            |
| eth\_getBlockTransactionCountByHash   | Returns transaction count in a block by hash   |
| eth\_getBlockTransactionCountByNumber | Returns transaction count in a block by number |

### Transaction Methods

| Method                                   | Description                                          |
| ---------------------------------------- | ---------------------------------------------------- |
| eth\_sendRawTransaction                  | Submits a signed transaction to the network          |
| eth\_getTransactionByHash                | Returns transaction details by hash                  |
| eth\_getTransactionByBlockHashAndIndex   | Returns transaction by block hash and index          |
| eth\_getTransactionByBlockNumberAndIndex | Returns transaction by block number and index        |
| eth\_getTransactionCount                 | Returns the nonce (transaction count) for an address |
| eth\_getTransactionReceipt               | Returns the receipt of a mined transaction           |

### Gas Methods

| Method           | Description                             |
| ---------------- | --------------------------------------- |
| eth\_gasPrice    | Returns the current L2 gas price in wei |
| eth\_estimateGas | Estimates gas needed for a transaction  |

### Filter Methods

| Method                           | Description                               |
| -------------------------------- | ----------------------------------------- |
| eth\_getLogs                     | Returns logs matching filter criteria     |
| eth\_newFilter                   | Creates a new log filter                  |
| eth\_newBlockFilter              | Creates a filter for new blocks           |
| eth\_newPendingTransactionFilter | Creates a filter for pending transactions |
| eth\_getFilterChanges            | Returns filter changes since last poll    |
| eth\_getFilterLogs               | Returns all logs matching filter          |
| eth\_uninstallFilter             | Removes an existing filter                |

### Subscription Methods (WebSocket)

| Method           | Description                       |
| ---------------- | --------------------------------- |
| eth\_subscribe   | Creates a subscription for events |
| eth\_unsubscribe | Cancels an existing subscription  |

### Uncle Methods

| Method                             | Description                              |
| ---------------------------------- | ---------------------------------------- |
| eth\_getUncleByBlockHashAndIndex   | Returns uncle block by hash and index    |
| eth\_getUncleByBlockNumberAndIndex | Returns uncle block by number and index  |
| eth\_getUncleCountByBlockHash      | Returns uncle count in a block by hash   |
| eth\_getUncleCountByBlockNumber    | Returns uncle count in a block by number |

### Network/Chain Methods

| Method              | Description                       |
| ------------------- | --------------------------------- |
| eth\_chainId        | Returns the chain ID (10)         |
| eth\_syncing        | Returns sync status of the node   |
| net\_version        | Returns the network ID            |
| net\_listening      | Returns listening status          |
| net\_peerCount      | Returns number of connected peers |
| web3\_clientVersion | Returns the client version        |
| web3\_sha3          | Returns Keccak-256 hash of data   |
| rpc\_modules        | Returns enabled RPC modules       |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Optimism Documentation](https://docs.optimism.io/)
* [Optimism Block Explorer (Optimistic Etherscan)](https://optimistic.etherscan.io/)
* [Optimism Bridge](https://app.optimism.io/bridge)
* [Ethereum JSON-RPC Specification](https://ethereum.org/developers/docs/apis/json-rpc/)
* [Optimism Website](https://www.optimism.io/)
