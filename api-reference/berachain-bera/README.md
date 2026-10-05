---
description: >-
  GetBlock provides fast and reliable access to Berachain nodes via JSON-RPC
  API. Connect to the Berachain network without running your own infrastructure.
---

# Berachain (BERA)

Berachain is an EVM-identical Layer 1 blockchain built on a novel Proof-of-Liquidity (PoL) consensus mechanism. Its execution layer is Bera-Reth, a Reth-based Ethereum client, so Berachain is bytecode-identical to Ethereum — standard contracts, the `eth_*` JSON-RPC surface, and tooling such as Foundry, Hardhat, Ethers.js, and Viem work without any modification. Consensus is provided by BeaconKit over CometBFT, giving fast, single-slot deterministic finality. Berachain uses a three-token model: BERA is the native gas token, BGT is the (non-transferable) governance token earned by providing liquidity and used to direct validator reward emissions, and HONEY is the native stablecoin. This reference document describes the Ethereum-compatible `eth_*` JSON-RPC interface, available over HTTP and WebSocket.

### Key Features

* **EVM Identical**: Bera-Reth is a Reth execution client, so Berachain runs Ethereum bytecode and tooling unchanged
* **Proof of Liquidity**: Validators are rewarded based on the liquidity directed to them via BGT, aligning security with on-chain liquidity
* **Three-Token Model**: BERA (gas), BGT (governance earned through liquidity), and HONEY (native stablecoin)
* **Fast Finality**: BeaconKit over CometBFT provides single-slot, deterministic finality with no reorgs
* **EIP-1559 Fees**: Base-fee pricing with `eth_feeHistory` and priority fees; native gas token BERA (18 decimals)
* **Full Ethereum RPC**: The complete `eth_*` / `net_*` / `web3_*` / `debug_*` surface, served over HTTP and WebSocket

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. Berachain implements the standard Ethereum JSON-RPC interface; the canonical specification for these methods is the Ethereum JSON-RPC specification at_ [_ethereum.org_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_, and Berachain-specific behaviour is documented at_ [_docs.berachain.com_](https://docs.berachain.com/)_._
{% endhint %}

## Network Information

| Property        | Value                                     |
| --------------- | ----------------------------------------- |
| Network Name    | Berachain                                 |
| Chain ID        | 80094                                     |
| Native Currency | BERA                                      |
| EVM Compatible  | Yes (EVM-identical, Bera-Reth)            |
| Consensus       | Proof-of-Liquidity (BeaconKit / CometBFT) |
| Block Time      | \~2 seconds                               |
| Finality        | Single-slot, deterministic                |
| Fee Model       | EIP-1559                                  |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="New York, USA" %}
```
https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Singapore, Singapore" %}
```
https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network | Chain ID | JSON-RPC | WSS | GraphQL | MEV protected (WebSocket) | MEV protected (JSON-RPC) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | -------- | -------- | --- | ------- | ------------------------- | ------------------------ | ------------------ | ------------- | -------------------- |
| Mainnet | 80094    | ✅        | ✅   | ❌       | ❌                         | ❌                        | ✅                  | ✅             | ✅                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

{% code overflow="wrap" %}
```bash
mkdir berachain-api-quickstart && cd berachain-api-quickstart && npm init --yes
```
{% endcode %}
{% endstep %}

{% step %}
### Install dependency

```bash
npm install axios
```
{% endstep %}

{% step %}
### Add code

{% code title="index.js" %}
```javascript
const axios = require('axios');

const url = 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  method: 'eth_blockNumber',
  params: [],
  id: 'getblock.io'
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => {
  console.log('Latest block:', parseInt(response.data.result, 16));
})
.catch(error => console.error(error));
```
{% endcode %}
{% endstep %}

{% step %}
### Run the script

```bash
node index.js
```
{% endstep %}

{% step %}
### Response

{% code overflow="wrap" %}
```bash
Latest Block: 27091663
```
{% endcode %}
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python(Request)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir berachain-api-quickstart && cd berachain-api-quickstart
python -m venv venv && source venv/bin/activate
```
{% endstep %}

{% step %}
### Install dependency

```bash
pip install requests
```
{% endstep %}

{% step %}
### Add code

{% code title="main.py" %}
```python
import requests
import json

url = "https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/"

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

### Chain & Client Info

| Method              | Description                                       |
| ------------------- | ------------------------------------------------- |
| web3\_clientVersion | Client software version identifier                |
| web3\_sha3          | Keccak-256 hash of hex-encoded data               |
| net\_version        | Network ID as a decimal string                    |
| net\_listening      | Whether the node is listening for P2P connections |
| net\_peerCount      | Number of connected peers                         |
| eth\_chainId        | Chain ID per EIP-155                              |
| eth\_blockNumber    | Current chain tip block number                    |
| eth\_syncing        | Sync status, or false if fully synced             |

### Gas & Fees

| Method                    | Description                                       |
| ------------------------- | ------------------------------------------------- |
| eth\_gasPrice             | Legacy gas price estimate in wei                  |
| eth\_maxPriorityFeePerGas | Suggested EIP-1559 priority fee in wei            |
| eth\_blobBaseFee          | Current blob base fee for EIP-4844 transactions   |
| eth\_feeHistory           | Historical base fees and priority-fee percentiles |

### Account & State

| Method                   | Description                                   |
| ------------------------ | --------------------------------------------- |
| eth\_getBalance          | ETH balance of an address at a given block    |
| eth\_accounts            | Accounts owned by the client                  |
| eth\_getStorageAt        | Value at a specific contract storage slot     |
| eth\_getTransactionCount | Address nonce (transaction count)             |
| eth\_getCode             | Deployed bytecode at an address               |
| eth\_getProof            | Merkle-Patricia proof for account and storage |

### Blocks

| Method                                | Description                             |
| ------------------------------------- | --------------------------------------- |
| eth\_getBlockByHash                   | Block data by block hash                |
| eth\_getBlockByNumber                 | Block data by block number or tag       |
| eth\_getBlockTransactionCountByHash   | Transaction count in a block, by hash   |
| eth\_getBlockTransactionCountByNumber | Transaction count in a block, by number |
| eth\_getBlockReceipts                 | All transaction receipts for a block    |

### Transactions

| Method                                   | Description                                    |
| ---------------------------------------- | ---------------------------------------------- |
| eth\_getTransactionByHash                | Transaction data by transaction hash           |
| eth\_getTransactionByBlockHashAndIndex   | Transaction by block hash and index            |
| eth\_getTransactionByBlockNumberAndIndex | Transaction by block number and index          |
| eth\_getTransactionReceipt               | Transaction receipt with status, gas, and logs |

### Execution & Simulation

| Method                | Description                                       |
| --------------------- | ------------------------------------------------- |
| eth\_call             | Execute a message call without a transaction      |
| eth\_estimateGas      | Estimate gas required to execute a transaction    |
| eth\_createAccessList | Compute an EIP-2930 access list for a transaction |
| eth\_simulateV1       | Simulate a bundle of transactions against a block |

### Filters & Logs

| Method                           | Description                                         |
| -------------------------------- | --------------------------------------------------- |
| eth\_newFilter                   | Create a log filter with address and topic criteria |
| eth\_newBlockFilter              | Create a filter for new block hashes                |
| eth\_newPendingTransactionFilter | Create a filter for pending transaction hashes      |
| eth\_uninstallFilter             | Remove a previously created filter                  |
| eth\_getFilterChanges            | Poll new entries for a filter since the last poll   |
| eth\_getFilterLogs               | Get all logs matching a log filter                  |
| eth\_getLogs                     | Query logs matching criteria in one call            |

### Transaction Submission

| Method                  | Description                                |
| ----------------------- | ------------------------------------------ |
| eth\_sendRawTransaction | Submit a signed transaction to the network |

### WebSocket Subscriptions

| Method           | Description                                                     |
| ---------------- | --------------------------------------------------------------- |
| eth\_subscribe   | Subscribe to newHeads, logs, newPendingTransactions, or syncing |
| eth\_unsubscribe | Cancel an active subscription                                   |

### RPC Module Discovery

| Method       | Description                                   |
| ------------ | --------------------------------------------- |
| rpc\_modules | List available RPC modules and their versions |

### Debug & Trace

| Method                         | Description                                     |
| ------------------------------ | ----------------------------------------------- |
| debug\_accountRange            | Enumerate accounts from the state trie          |
| debug\_batchSendRawTransaction | Submit multiple signed transactions in one call |
| debug\_getBadBlocks            | List recently rejected invalid blocks           |
| debug\_storageRangeAt          | Enumerate storage slots of a contract           |
| debug\_traceBlock              | Trace execution of a block by RLP               |
| debug\_traceBlockByHash        | Trace all transactions in a block by hash       |
| debug\_traceBlockByNumber      | Trace all transactions in a block by number     |
| debug\_traceCall               | Trace a simulated message call                  |
| debug\_traceTransaction        | Trace execution of a mined transaction          |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Berachain Documentation](https://docs.berachain.com/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [Berascan Explorer](https://berascan.com/)
* [Berachain Website](https://berachain.com/)
