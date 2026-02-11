---
description: >-
  GetBlock provides fast and reliable access to Sei nodes via JSON-RPC API.
  Connect to the Sei network without running your own infrastructure.
---

# SEI

Sei is a high-performance Layer 1 blockchain and the first parallelized EVM, designed to deliver Web2-like experiences for decentralized applications. Built on the Cosmos SDK with Twin Turbo Consensus, Sei achieves sub-400 millisecond finality and can process up to 200,000 transactions per second. The network features optimistic parallelization that allows transactions to execute simultaneously, SeiDB for optimized storage, and full EVM compatibility enabling seamless deployment of Ethereum smart contracts. Sei also supports cross-VM interoperability between EVM and CosmWasm environments.

### Key Features

* **First Parallelized EVM**: Optimistic parallelization enables concurrent transaction execution without developer-defined dependencies
* **Ultra-Fast Finality**: Twin Turbo Consensus achieves \~400ms block times with instant finality
* **High Throughput**: Capable of processing up to 200,000 TPS with 100 megagas per second
* **Full EVM Compatibility**: Deploy Ethereum smart contracts without modification using familiar tools
* **Cross-VM Interoperability**: Seamless composability between EVM and CosmWasm environments
* **SeiDB Storage**: Optimized database architecture for faster reads/writes and reduced state bloat
* **Sub-Cent Fees**: Extremely low transaction costs for users
* **SEI Native Token**: Used for gas fees, staking, and governance (18 decimals)
* **Cosmos Ecosystem**: Built on Cosmos SDK with IBC support for cross-chain communication

{% hint style="info" %}
**TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION**

GetBlock's RPC API reference documentation is provided exclusively for informational purposes and streamlined developer experience optimization. The canonical and normative specification for Ethereum Virtual Machine (EVM) JSON-RPC methods is solely maintained and published through the official Ethereum Foundation documentation portal at [ethereum.org](https://ethereum.org/). This resource constitutes the sole authoritative reference implementation of the JSON-RPC 2.0 protocol interface across EVM-compatible execution clients.
{% endhint %}

## Network Information

| Property        | Value                      |
| --------------- | -------------------------- |
| Network Name    | Sei Mainnet                |
| Chain ID        | 1329                       |
| Native Currency | SEI                        |
| Decimals        | 18                         |
| Network Type    | Layer 1 (Parallelized EVM) |
| Block Time      | \~400 milliseconds         |
| Finality        | Instant (\~400ms)          |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://go.getblock.io
```
{% endtab %}

{% tab title="Singapore, Singapore" %}
```
https://go.getblock.asia
```
{% endtab %}

{% tab title="New York, USA" %}
```
https://go.getblock.us
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network | Chain ID | JSON RPC | WSS |
| ------- | -------- | -------- | --- |
| Mainnet | 1329     | ✅        | ✅   |

## Quickstart

In this section, you will learn how to make your first call with either:

* Axios
* Python

Before you begin, you must have already installed [`npm`](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [`yarn`](https://classic.yarnpkg.com/lang/en/docs/install) on your local machine (for the Axios example) or Python and pip (for the Python example).

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

Create and initialize a new project:

```bash
mkdir sei-api-quickstart
cd sei-api-quickstart
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
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
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
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
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
    "result": "0xb6fd83a"
}
```
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python(Request)" %}
{% stepper %}
{% step %}
### Setup the project directory

```bash
mkdir sei-api-quickstart
cd sei-api-quickstart
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

GetBlock provides access to standard Ethereum JSON-RPC methods and Sei-specific extensions for the Sei network.

#### Transaction Methods

| Method                                   | Description                                   |
| ---------------------------------------- | --------------------------------------------- |
| eth\_getTransactionByHash                | Returns transaction by hash                   |
| eth\_getTransactionByBlockHashAndIndex   | Returns transaction by block hash and index   |
| eth\_getTransactionByBlockNumberAndIndex | Returns transaction by block number and index |
| eth\_getTransactionCount                 | Returns the transaction count (nonce)         |
| eth\_getTransactionReceipt               | Returns the receipt of a transaction          |
| eth\_sendRawTransaction                  | Submits a signed transaction                  |

#### Block Methods

| Method                                | Description                               |
| ------------------------------------- | ----------------------------------------- |
| eth\_blockNumber                      | Returns the current block number          |
| eth\_getBlockByHash                   | Returns block by hash                     |
| eth\_getBlockByNumber                 | Returns block by number                   |
| eth\_getBlockReceipts                 | Returns all receipts for a block          |
| eth\_getBlockTransactionCountByHash   | Returns transaction count by block hash   |
| eth\_getBlockTransactionCountByNumber | Returns transaction count by block number |

#### Account/State Methods

| Method            | Description                                    |
| ----------------- | ---------------------------------------------- |
| eth\_getBalance   | Returns the SEI balance of an address          |
| eth\_getStorageAt | Returns the value at a storage position        |
| eth\_getCode      | Returns the code at an address                 |
| eth\_call         | Executes a call without creating a transaction |
| eth\_getProof     | Returns account and storage proof              |
| eth\_accounts     | Returns a list of addresses owned by client    |

#### Gas and Fee Methods

| Method           | Description                        |
| ---------------- | ---------------------------------- |
| eth\_gasPrice    | Returns the current gas price      |
| eth\_estimateGas | Estimates gas for a transaction    |
| eth\_feeHistory  | Returns historical gas information |

#### Filter Methods

| Method                | Description                            |
| --------------------- | -------------------------------------- |
| eth\_getLogs          | Returns logs matching filter criteria  |
| eth\_newFilter        | Creates a log filter                   |
| eth\_newBlockFilter   | Creates a block filter                 |
| eth\_getFilterChanges | Returns filter changes since last poll |
| eth\_getFilterLogs    | Returns all logs matching filter       |
| eth\_uninstallFilter  | Removes a filter                       |

#### Network/Chain Methods

| Method              | Description                       |
| ------------------- | --------------------------------- |
| eth\_chainId        | Returns the chain ID (1329)       |
| eth\_coinbase       | Returns the fee-collector address |
| eth\_syncing        | Returns sync status               |
| net\_version        | Returns the network ID            |
| web3\_clientVersion | Returns client version            |

#### Debug Methods

| Method                    | Description                                |
| ------------------------- | ------------------------------------------ |
| debug\_traceBlockByHash   | Traces all transactions in a block by hash |
| debug\_traceBlockByNumber | Traces block by number                     |
| debug\_traceTransaction   | Traces a specific transaction              |

#### Sei-Specific Methods

| Method                                     | Description                                              |
| ------------------------------------------ | -------------------------------------------------------- |
| sei\_associate                             | Associates Sei address with EVM address                  |
| sei\_getBlockReceipts                      | Returns block receipts including synthetic transactions  |
| sei\_getLogs                               | Returns logs including synthetic logs from Cosmos events |
| sei\_getFilterLogs                         | Returns filter logs including synthetic logs             |
| sei\_traceBlockByNumberExcludeTraceFail    | Traces block excluding failed traces                     |
| sei\_getTransactionReceiptExcludeTraceFail | Returns receipt excluding failed traces                  |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Sei Developer Docs](https://docs.sei.io/)
* [Sei Explorer (Seiscan)](https://seiscan.io/)
* [Sei Explorer (SeiTrace)](https://seitrace.com/)
* [Sei Bridge](https://app.sei.io/bridge)
* [Ethereum JSON RPC API](https://ethereum.org/developers/docs/apis/json-rpc/)
* [Sei Website](https://sei.io/)
