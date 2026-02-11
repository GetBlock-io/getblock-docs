---
description: >-
  GetBlock provides fast and reliable access to BNB Smart Chain nodes via
  JSON-RPC API. Connect to the BSC network without running your own
  infrastructure.
---

# BNB Smart Chain (BSC)

BNB Smart Chain (BSC) is a high-performance, EVM-compatible blockchain launched by Binance in September 2020. Designed to run alongside BNB Beacon Chain, BSC enables the creation and deployment of smart contracts and decentralized applications (DApps) with low transaction fees and fast block times. The network uses Proof of Staked Authority (PoSA) consensus, combining delegated Proof of Stake and Proof of Authority for high throughput and decentralization.

### Key Features&#x20;

* **EVM Compatibility**: Full Ethereum Virtual Machine compatibility for seamless dApp migration
* **Fast Block Times**: \~3 second block production with \~6 second finality
* **Low Transaction Fees**: Average fees around $0.10, significantly lower than Ethereum
* **Proof of Staked Authority (PoSA)**: Hybrid consensus combining PoS and PoA
* **55 Validators**: Decentralized validator set with staking-based governance
* **BNB Token**: Native token for gas fees, staking, and governance (18 decimals)
* **Rich Ecosystem**: 1,500+ DApps including PancakeSwap, Venus, and BakerySwap
* **Cross-Chain Compatibility**: Interoperability with BNB Beacon Chain
* **Developer-Friendly**: Compatible with MetaMask, Truffle, Remix, and other Ethereum tools

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._&#x20;

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for Ethereum Virtual Machine (EVM) JSON-RPC methods is solely maintained and published through the official Ethereum Foundation documentation portal at_ [_ethereum.org_](http://ethereum.org/)_. This resource constitutes the sole authoritative reference implementation of the JSON-RPC 2.0 protocol interface across EVM-compatible execution clients._
{% endhint %}

## Network Information

| Property        | Value                            |
| --------------- | -------------------------------- |
| Network Name    | BNB Smart Chain Mainnet          |
| Chain ID        | 56                               |
| Native Currency | BNB                              |
| Decimals        | 18                               |
| Block Time      | \~3 seconds                      |
| Consensus       | Proof of Staked Authority (PoSA) |
| EVM Compatible  | Yes                              |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="New York, USA" %}
```
https://go.getblock.us/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Singapore, Singapore" %}
```
https://go.getblock.asia/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network | Chain ID | JSON-RPC | WSS | GraphQL | MEV protected (WebSocket) | MEV protected (JSON-RPC) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | -------- | -------- | --- | ------- | ------------------------- | ------------------------ | ------------------ | ------------- | -------------------- |
| Mainnet | 56       | ✅        | ✅   | ✅       | ✅                         | ✅                        | ✅                  | ✅             | ✅                    |
| Testnet | 97       | ✅        | ✅   | ✅       | ❌                         | ❌                        | ✅                  | ❌             | ❌                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

Create and initialize a new project:

```bash
mkdir bsc-api-quickstart
cd bsc-api-quickstart
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

Add the following code to `index.js`:

{% code title="index.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

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
  const blockNumber = parseInt(response.data.result, 16);
  console.log('Current Block Number:', blockNumber);
})
.catch(error => console.error(error));
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
{% endstep %}

{% step %}
### Run the script

```bash
node index.js
```

Expected output (example):

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x4a87a53"
}
```
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python(Request)" %}
{% stepper %}
{% step %}
### Set up the project directory

```bash
mkdir bsc-api-quickstart
cd bsc-api-quickstart
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
### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

## Available API Methods

### Transaction Methods

| Method                                   | Description                                     |
| ---------------------------------------- | ----------------------------------------------- |
| eth\_sendRawTransaction                  | Submits a signed transaction                    |
| eth\_sendTransaction                     | Sends a transaction (requires unlocked account) |
| eth\_getTransactionByHash                | Returns transaction by hash                     |
| eth\_getTransactionByBlockHashAndIndex   | Returns transaction by block hash and index     |
| eth\_getTransactionByBlockNumberAndIndex | Returns transaction by block number and index   |
| eth\_getTransactionCount                 | Returns the transaction count (nonce)           |
| eth\_getTransactionReceipt               | Returns the receipt of a transaction            |

### Block Methods

| Method                                | Description                               |
| ------------------------------------- | ----------------------------------------- |
| eth\_blockNumber                      | Returns the current block number          |
| eth\_getBlockTransactionCountByHash   | Returns transaction count by block hash   |
| eth\_getBlockTransactionCountByNumber | Returns transaction count by block number |
| eth\_getUncleByBlockHashAndIndex      | Returns uncle by block hash and index     |
| eth\_getUncleByBlockNumberAndIndex    | Returns uncle by block number and index   |
| eth\_getUncleCountByBlockHash         | Returns uncle count by block hash         |
| eth\_getUncleCountByBlockNumber       | Returns uncle count by block number       |

### Account/State Methods

| Method            | Description                                    |
| ----------------- | ---------------------------------------------- |
| eth\_accounts     | Returns addresses owned by client              |
| eth\_getBalance   | Returns the BNB balance of an address          |
| eth\_getStorageAt | Returns the value at a storage position        |
| eth\_getCode      | Returns the code at an address                 |
| eth\_getProof     | Returns account and storage proof              |
| eth\_call         | Executes a call without creating a transaction |
| eth\_sign         | Signs data with account                        |

### Gas and Fee Methods

| Method                    | Description                        |
| ------------------------- | ---------------------------------- |
| eth\_gasPrice             | Returns the current gas price      |
| eth\_estimateGas          | Estimates gas for a transaction    |
| eth\_feeHistory           | Returns historical gas information |
| eth\_maxPriorityFeePerGas | Returns current max priority fee   |

### Filter Methods

| Method                           | Description                            |
| -------------------------------- | -------------------------------------- |
| eth\_getLogs                     | Returns logs matching filter criteria  |
| eth\_newFilter                   | Creates a log filter                   |
| eth\_newBlockFilter              | Creates a block filter                 |
| eth\_newPendingTransactionFilter | Creates a pending transaction filter   |
| eth\_getFilterChanges            | Returns filter changes since last poll |
| eth\_getFilterLogs               | Returns all logs matching filter       |
| eth\_uninstallFilter             | Removes a filter                       |

### Subscription Methods (WebSocket)

| Method           | Description                      |
| ---------------- | -------------------------------- |
| eth\_unsubscribe | Cancels an existing subscription |

### Network/Chain Methods

| Method              | Description                       |
| ------------------- | --------------------------------- |
| eth\_chainId        | Returns the chain ID (56)         |
| eth\_syncing        | Returns sync status               |
| eth\_coinbase       | Returns the coinbase address      |
| eth\_mining         | Returns mining status             |
| net\_version        | Returns the network ID            |
| net\_listening      | Returns listening status          |
| net\_peerCount      | Returns number of connected peers |
| web3\_clientVersion | Returns client version            |
| web3\_sha3          | Returns Keccak-256 hash of data   |

### Transaction Pool Methods

| Method         | Description                     |
| -------------- | ------------------------------- |
| txpool\_status | Returns transaction pool status |

### Debug Methods

| Method                             | Description                   |
| ---------------------------------- | ----------------------------- |
| debug\_traceTransaction            | Traces a specific transaction |
| debug\_traceBlockByHash            | Traces block by hash          |
| debug\_traceBlockByNumber          | Traces block by number        |
| debug\_traceCall                   | Traces a call                 |
| debug\_traceBlock                  | Traces a block                |
| debug\_accountRange                | Returns accounts in range     |
| debug\_storageRangeAt              | Returns storage range         |
| debug\_standardTraceBadBlockToFile | Traces bad block to file      |
| debug\_standardTraceBlockToFile    | Traces block to file          |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [BNB Chain Documentation](https://docs.bnbchain.org/)
* [BscScan Block Explorer](https://bscscan.com/)
* [BNB Chain Website](https://www.bnbchain.org/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/developers/docs/apis/json-rpc/)
