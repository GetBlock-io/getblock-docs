---
description: >-
  GetBlock provides fast and reliable access to World Chain nodes via JSON-RPC
  API. Connect to the World Chain network without running your own
  infrastructure.
---

# WorldChain

World Chain is an Ethereum Layer 2 blockchain built on the OP Stack, designed with humans at its core. Co-founded by Sam Altman and Alex Blania as part of the World (formerly Worldcoin) ecosystem, it launched in October 2024 as a member of the Optimism Superchain. The network features 2-second block times, low transaction fees, and a unique human-first approach that provides verified World ID holders with free gas allowances and priority blockspace access. World Chain is fully EVM-compatible, enabling seamless deployment of existing Ethereum smart contracts.

### Key Features

* **Ethereum Layer 2**: Built on OP Stack, leveraging Ethereum's security with fast, low-cost transactions
* **Human-First Design**: World ID verified users receive free gas allowances and priority transaction inclusion
* **2-Second Block Times**: Fast block production for responsive dApp experiences
* **Optimism Superchain**: Part of the Superchain ecosystem, enabling cross-chain interoperability
* **Low Transaction Fees**: Optimized L2 costs significantly lower than Ethereum mainnet
* **Full EVM Compatibility**: Deploy Ethereum smart contracts without modification
* **ETH Native Token**: Uses ETH for gas fees (18 decimals)
* **World ID Integration**: Native support for privacy-preserving proof of personhood
* **Priority Blockspace**: Verified humans get guaranteed block space allocation

{% hint style="info" %}
**TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION**

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and streamlined developer experience optimization. The canonical and normative specification for Ethereum Virtual Machine (EVM) JSON-RPC methods is solely maintained and published through the official Ethereum Foundation documentation portal at_ [_ethereum.org_](https://ethereum.org/)_. This resource constitutes the sole authoritative reference implementation of the JSON-RPC 2.0 protocol interface across EVM-compatible execution clients._
{% endhint %}

## Network Information

| Property        | Value                       |
| --------------- | --------------------------- |
| Network Name    | World Chain Mainnet         |
| Chain ID        | 480                         |
| Native Currency | ETH                         |
| Decimals        | 18                          |
| Network Type    | Ethereum Layer 2 (OP Stack) |
| Block Time      | \~2 seconds                 |
| Gas Limit       | 80,000,000                  |

## Base URL

{% tabs %}
{% tab title="New York, USA" %}
```
https://go.getblock.us
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network | Chain ID | JSON RPC | WSS |
| ------- | -------- | -------- | --- |
| Mainnet | 480      | ✅        | ✅   |

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
mkdir worldchain-api-quickstart
cd worldchain-api-quickstart
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
    url: 'https://go.getblock.us/<ACCESS-TOKEN>/',
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
    "result": "0x18209e3"
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
mkdir worldchain-api-quickstart
cd worldchain-api-quickstart
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

url = "https://go.getblock.us/<ACCESS-TOKEN>/"

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

GetBlock provides access to standard Ethereum JSON-RPC methods for the World Chain network.

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
| eth\_getBalance   | Returns the ETH balance of an address          |
| eth\_getStorageAt | Returns the value at a storage position        |
| eth\_getCode      | Returns the code at an address                 |
| eth\_call         | Executes a call without creating a transaction |
| eth\_getProof     | Returns account and storage proof              |
| eth\_accounts     | Returns a list of addresses owned by client    |

#### Gas and Fee Methods

| Method                    | Description                        |
| ------------------------- | ---------------------------------- |
| eth\_gasPrice             | Returns the current gas price      |
| eth\_estimateGas          | Estimates gas for a transaction    |
| eth\_feeHistory           | Returns historical gas information |
| eth\_maxPriorityFeePerGas | Returns current max priority fee   |

#### Filter Methods

| Method                | Description                            |
| --------------------- | -------------------------------------- |
| eth\_getLogs          | Returns logs matching filter criteria  |
| eth\_newFilter        | Creates a log filter                   |
| eth\_newBlockFilter   | Creates a block filter                 |
| eth\_getFilterChanges | Returns filter changes since last poll |
| eth\_getFilterLogs    | Returns all logs matching filter       |
| eth\_uninstallFilter  | Removes a filter                       |

#### Subscription Methods (WebSocket)

| Method           | Description                       |
| ---------------- | --------------------------------- |
| eth\_subscribe   | Creates a subscription for events |
| eth\_unsubscribe | Cancels an existing subscription  |

#### Network/Chain Methods

| Method              | Description                       |
| ------------------- | --------------------------------- |
| eth\_chainId        | Returns the chain ID (480)        |
| eth\_syncing        | Returns sync status               |
| net\_version        | Returns the network ID            |
| net\_listening      | Returns listening status          |
| net\_peerCount      | Returns number of connected peers |
| web3\_clientVersion | Returns client version            |
| web3\_sha3          | Returns Keccak-256 hash of data   |

#### Debug Methods

| Method                    | Description                                |
| ------------------------- | ------------------------------------------ |
| debug\_traceBlockByHash   | Traces all transactions in a block by hash |
| debug\_traceBlockByNumber | Traces block by number                     |
| debug\_traceTransaction   | Traces a specific transaction              |
| debug\_traceCall          | Traces a call without creating transaction |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [World Chain Developer Docs](https://docs.world.org/world-chain)
* [World Chain Explorer](https://worldscan.org/)
* [World Chain Bridge](https://worldchain-mainnet.bridge.alchemy.com/)
* [Ethereum JSON RPC API](https://ethereum.org/developers/docs/apis/json-rpc/)
* [World Website](https://world.org/)
