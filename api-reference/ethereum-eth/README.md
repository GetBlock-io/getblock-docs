---
description: >-
  GetBlock provides fast and reliable access to Ethereum nodes via JSON-RPC API.
  Connect to the Ethereum network without running your own infrastructure.
---

# Ethereum(ETH)

Ethereum is the largest smart-contract platform by total value settled, secured by a proof-of-stake consensus layer of over one million validators. Built by the Ethereum Foundation and a broad developer community, it is the reference L1 execution environment for the EVM and settles the majority of Layer-2 rollup activity, DeFi TVL, and stablecoin volume across all blockchains. As of the Fusaka upgrade (December 2025), the network operates with a 60M gas limit, up to 21 blobs per block via EIP-4844 + PeerDAS, and EOA-to-contract delegation via EIP-7702 (Pectra, May 2025).

### Key Features

* **Proof-of-Stake Consensus**: Secured by \~1M validators via the beacon chain since The Merge (September 2022)
* **12-Second Block Time**: One block per slot, with finality reached in two epochs (\~13 minutes) under normal conditions
* **EIP-1559 Fee Market**: Base fee + priority tip pricing with dynamic base-fee adjustment per block
* **EIP-4844 Blob Data Availability**: Post-Fusaka, up to 21 blobs per block enable L2 rollups to post compressed batches at low cost
* **EIP-7702 Account Delegation**: EOAs can temporarily execute smart-contract code, unlocking batching, gas sponsorship, and social recovery without wallet migration

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._&#x20;

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for Ethereum Virtual Machine (EVM) JSON-RPC methods is maintained and published solely through the official Ethereum Foundation documentation portal at_ [_ethereum.org_](http://ethereum.org/)_. This resource is the sole authoritative reference implementation of the JSON-RPC 2.0 protocol for EVM-compatible execution clients._
{% endhint %}

### Network Information

| Property        | Value                |
| --------------- | -------------------- |
| Network Name    | Ethereum             |
| Chain ID        | 1                    |
| Native Currency | ETH                  |
| Decimals        | 18                   |
| Block Time      | \~3 seconds          |
| Consensus       | Proof of Stake (PoS) |
| EVM Compatible  | Yes                  |

### Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="New York, USA" %}
```bash
https://shared.us-east-1.getblock.io/<ACCESS_TOKEN>
```
{% endtab %}

{% tab title="Singapore, Singapore" %}
```bash
https://shared.ap-southeast-1.getblock.io/<ACCESS_TOKEN>
```
{% endtab %}
{% endtabs %}

### Supported Networks

| Network | Chain ID | JSON-RPC | WSS | GraphQL | MEV protected (WebSocket) | MEV protected (JSON-RPC) | MEV protected -RElay (JSON\_RPC) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | -------- | -------- | --- | ------- | ------------------------- | ------------------------ | -------------------------------- | ------------------ | ------------- | -------------------- |
| Mainnet | 1        | ✅        | ✅   | ✅       | ✅                         | ✅                        | ✅                                | ✅                  | ✅             | ✅                    |
| Hoodi   | 17000    | ✅        | ✅   | ✅       | ❌                         | ❌                        | ❌                                | ✅                  | ✅             | ✅                    |
| Sepolia | 11155111 | ✅        | ✅   | ✅       | ❌                         | ❌                        | ❌                                | ✅                  | ❌             | ❌                    |

### Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
#### Setup project

Create and initialize a new project:

```bash
mkdir eth-api-quickstart
cd eth-api-quickstart
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
const axios = require('axios');

const url = 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  method: 'eth_chainId',
  params: [],
  id: 'getblock.io'
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => {
  const blockNumber = parseInt(response.data.result, 16);
  console.log('Current Block Number:', blockNumber);
})
.catch(error => console.error(error));
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual GetBlock access token.
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
    "result": "0x1",
    "id": "getblock.io"
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
mkdir eth-api-quickstart
cd eth-api-quickstart
```
{% endstep %}

{% step %}
#### Create and activate a virtual environment

```bash
python -m venv venv
source venv/bin/activate

# On Windows, use:
venv\Scripts\activate
```
{% endstep %}

{% step %}
#### Install requests

```bash
pip install requests
```
{% endstep %}

{% step %}
### Create script

Create a file called `main.py` with the following content:

{% code title="main.py" %}
```python
import requests
import json

url = "https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/"

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

### Available Methods

#### Chain & Client Info

| Method               | Description                                       |
| -------------------- | ------------------------------------------------- |
| `web3_clientVersion` | Client software version identifier                |
| `web3_sha3`          | Keccak-256 hash of hex-encoded data               |
| `net_version`        | Network ID (decimal string)                       |
| `net_listening`      | Whether the node is listening for P2P connections |
| `net_peerCount`      | Number of connected peers                         |
| `eth_chainId`        | Chain ID per EIP-155                              |
| `eth_blockNumber`    | Current chain tip block number                    |
| `eth_syncing`        | Sync status (or `false` if fully synced)          |

#### Gas & Fees

| Method                     | Description                                          |
| -------------------------- | ---------------------------------------------------- |
| `eth_gasPrice`             | Legacy gas price estimate (wei)                      |
| `eth_maxPriorityFeePerGas` | Suggested EIP-1559 priority fee (wei)                |
| `eth_blobBaseFee`          | Current blob base fee for EIP-4844 blob transactions |
| `eth_feeHistory`           | Historical base fees + priority-fee percentiles      |

#### Account & State

| Method                    | Description                                               |
| ------------------------- | --------------------------------------------------------- |
| `eth_getBalance`          | ETH balance of an address at a given block                |
| `eth_accounts`            | Accounts owned by the client (returns `[]` on public RPC) |
| `eth_getStorageAt`        | Value at a specific storage slot of a contract            |
| `eth_getTransactionCount` | Address nonce (transaction count)                         |
| `eth_getCode`             | Deployed bytecode at an address                           |
| `eth_getProof`            | Merkle-Patricia proof for account and storage slots       |

#### Blocks

| Method                                 | Description                             |
| -------------------------------------- | --------------------------------------- |
| `eth_getBlockByHash`                   | Block data by block hash                |
| `eth_getBlockByNumber`                 | Block data by block number or tag       |
| `eth_getBlockTransactionCountByHash`   | Transaction count in a block, by hash   |
| `eth_getBlockTransactionCountByNumber` | Transaction count in a block, by number |
| `eth_getBlockReceipts`                 | All transaction receipts for a block    |

#### Transactions

| Method                                    | Description                             |
| ----------------------------------------- | --------------------------------------- |
| `eth_getTransactionByHash`                | Transaction data by transaction hash    |
| `eth_getTransactionByBlockHashAndIndex`   | Transaction by block hash and index     |
| `eth_getTransactionByBlockNumberAndIndex` | Transaction by block number and index   |
| `eth_getTransactionReceipt`               | Transaction receipt (status, gas, logs) |

#### Execution & Simulation

| Method                 | Description                                       |
| ---------------------- | ------------------------------------------------- |
| `eth_call`             | Execute a message call without a transaction      |
| `eth_estimateGas`      | Estimate gas required to execute a transaction    |
| `eth_createAccessList` | Compute an EIP-2930 access list for a transaction |
| `eth_simulateV1`       | Simulate a bundle of transactions against a block |

#### Filters & Logs

| Method                            | Description                                     |
| --------------------------------- | ----------------------------------------------- |
| `eth_newFilter`                   | Create a log filter with address+topic criteria |
| `eth_newBlockFilter`              | Create a filter for new block hashes            |
| `eth_newPendingTransactionFilter` | Create a filter for pending transaction hashes  |
| `eth_uninstallFilter`             | Remove a previously created filter              |
| `eth_getFilterChanges`            | Poll new entries for a filter since last poll   |
| `eth_getFilterLogs`               | Get all matching logs for a log filter          |
| `eth_getLogs`                     | Query logs matching criteria (one-shot)         |

#### Transaction Submission

| Method                   | Description                                |
| ------------------------ | ------------------------------------------ |
| `eth_sendRawTransaction` | Submit a signed transaction to the network |

#### WebSocket Subscriptions

| Method            | Description                                                             |
| ----------------- | ----------------------------------------------------------------------- |
| `eth_subscribe`   | Subscribe to `newHeads`, `logs`, `newPendingTransactions`, or `syncing` |
| `eth_unsubscribe` | Cancel a subscription                                                   |

#### RPC Module Discovery

| Method        | Description                                   |
| ------------- | --------------------------------------------- |
| `rpc_modules` | List available RPC modules and their versions |

### Debug & Trace

| Method                          | Description                                     |
| ------------------------------- | ----------------------------------------------- |
| `debug_accountRange`            | Enumerate accounts from the state trie          |
| `debug_batchSendRawTransaction` | Submit multiple signed transactions in one call |
| `debug_getBadBlocks`            | List recently rejected invalid blocks           |
| `debug_storageRangeAt`          | Enumerate storage slots of a contract           |
| `debug_traceBlock`              | Trace execution of a block by RLP               |
| `debug_traceBlockByHash`        | Trace all transactions in a block by hash       |
| `debug_traceBlockByNumber`      | Trace all transactions in a block by number     |
| `debug_traceCall`               | Trace a simulated message call                  |
| `debug_traceTransaction`        | Trace execution of a mined transaction          |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Ethereum JSON-RPC Specification](https://ethereum.org/developers/docs/apis/json-rpc/)&#x20;
