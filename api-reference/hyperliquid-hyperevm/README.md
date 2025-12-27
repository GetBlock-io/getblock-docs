---
description: >-
  Access the HyperEVM blockchain through JSON-RPC API. HyperEVM is the
  Ethereum-compatible smart contract layer of Hyperliquid, enabling direct
  integration with one of the most performant decentralized
---

# Hyperliquid (HyperEVM)

### Overview

HyperEVM is an Ethereum Virtual Machine (EVM) execution layer embedded within Hyperliquid's Layer 1 blockchain. It shares state with HyperCore (the trading engine) and is secured by the same HyperBFT consensus mechanism. This unique architecture allows smart contracts to directly access Hyperliquid's on-chain order books, spot trading, and perpetual markets.

#### Key Features

* **EVM Compatibility**: Full Ethereum tooling support (Foundry, Hardhat, ethers.js, web3.js)
* **Unified State**: Smart contracts can interact directly with HyperCore trading infrastructure
* **HyperBFT Consensus**: Sub-second finality with high throughput
* **EIP-1559 Support**: Cancun hardfork (without blobs) with base fee mechanism
* **Native HYPE Token**: Gas payments in HYPE with 18 decimals
* **Fee Burning**: Both base fees and priority fees are burned

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._&#x20;

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for Ethereum Virtual Machine (EVM) JSON-RPC methods is solely maintained and published through the official Ethereum Foundation documentation portal at_ [_ethereum.org_](http://ethereum.org/)_. This resource constitutes the sole authoritative reference implementation of the JSON-RPC 2.0 protocol interface across EVM-compatible execution clients._
{% endhint %}

### Supported Network

| Network | Chain ID | JSON | WSS |
| ------- | -------- | ---- | --- |
| Mainnet | 999      | ✔    | ✔   |

### Base URL

{% tabs %}
{% tab title="New York, USA" %}
```bash
https://go.getblock.us
```
{% endtab %}

{% tab title="Frankfurt, Germany" %}
```bash
https://go.getblock.io
```
{% endtab %}

{% tab title="Signapore, Signapore" %}
```bash
https://go.getblock.asia
```
{% endtab %}
{% endtabs %}

### Available API Methods

#### Standard Ethereum Methods

| Method                                   | Description                        | Limitations             |
| ---------------------------------------- | ---------------------------------- | ----------------------- |
| eth\_blockNumber                         | Get current block number           | -                       |
| eth\_chainId                             | Get chain ID                       | -                       |
| eth\_gasPrice                            | Get gas price for next small block | -                       |
| eth\_getBalance                          | Get account balance                | Latest block only       |
| eth\_getBlockByHash                      | Get block by hash                  | -                       |
| eth\_getBlockByNumber                    | Get block by number                | -                       |
| eth\_getBlockReceipts                    | Get all receipts in block          | -                       |
| eth\_getBlockTransactionCountByHash      | Get tx count by block hash         | -                       |
| eth\_getBlockTransactionCountByNumber    | Get tx count by block number       | -                       |
| eth\_getCode                             | Get contract bytecode              | Latest block only       |
| eth\_getLogs                             | Get event logs                     | Max 4 topics, 50 blocks |
| eth\_getStorageAt                        | Get storage value                  | Latest block only       |
| eth\_getTransactionByHash                | Get transaction by hash            | -                       |
| eth\_getTransactionByBlockHashAndIndex   | Get tx by block hash and index     | -                       |
| eth\_getTransactionByBlockNumberAndIndex | Get tx by block number and index   | -                       |
| eth\_getTransactionCount                 | Get account nonce                  | Latest block only       |
| eth\_getTransactionReceipt               | Get transaction receipt            | -                       |
| eth\_call                                | Execute call without transaction   | Latest block only       |
| eth\_estimateGas                         | Estimate gas for transaction       | Latest block only       |
| eth\_feeHistory                          | Get historical fee data            | -                       |
| eth\_maxPriorityFeePerGas                | Get priority fee suggestion        | Always returns 0        |
| eth\_syncing                             | Get sync status                    | Always returns false    |
| net\_version                             | Get network ID                     | -                       |
| web3\_clientVersion                      | Get client version                 | -                       |

#### HyperEVM-Specific Methods

| Method                         | Description                                       |
| ------------------------------ | ------------------------------------------------- |
| eth\_bigBlockGasPrice          | Get gas price for next big block                  |
| eth\_usingBigBlocks            | Check if address uses big blocks                  |
| eth\_getSystemTxsByBlockHash   | Get HyperCore system transactions by block hash   |
| eth\_getSystemTxsByBlockNumber | Get HyperCore system transactions by block number |

### Quickstart

In this section, you will learn how to make your first call with either:

* Axios
* Python

#### Quickstart with Axios

Before you begin, you must have already installed `npm` or `yarn` on your local machine. If not, check out [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [yarn](https://classic.yarnpkg.com/lang/en/docs/install).

1. Set up your project using this command:

{% tabs %}
{% tab title="npm" %}
```bash
mkdir hyperevm-api-quickstart
cd hyperevm-api-quickstart
npm init --yes
```
{% endtab %}

{% tab title="yarn" %}
```bash
mkdir hyperevm-api-quickstart
cd hyperevm-api-quickstart
yarn init --yes
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
This creates a project directory named `hyperevm-api-quickstart` and initialises a Node.js project within it.
{% endhint %}

2. Install Axios using this command:

{% tabs %}
{% tab title="npm" %}
```bash
npm install axios
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add axios
```
{% endtab %}
{% endtabs %}

3. Create a new file and name it `index.js`. This is where you will make your first call.
4. Set the ES module `"type": "module"` in your `package.json`.
5.  Add the following code to the file (`index.js`):

    ```js
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

    > Replace `<ACCESS_TOKEN>` with your actual access token from GetBlock.
6.  Run the script:

    ```bash
    node index.js
    ```

    The sequence number and authentication key log in your console like this:

    ```json
    {
        "jsonrpc": "2.0",
        "id": "getblock.io",
        "result": "0x15776ea"
    }
    ```

#### Quickstart with Python and Requests

Before you begin, you must have installed Python and Pip on your local machine.

1.  Set up your project using this command:

    ```bash
    mkdir arbitrum-api-quickstart
    cd arbitrum-api-quickstart
    ```
2.  Set up a virtual environment to isolate dependencies:

    ```bash
    python -m venv venv
    source venv/bin/activate  
    # On Windows, use venv\Scripts\activate
    ```
3.  Install the requests library:

    ```bash
    pip install requests
    ```
4.  Create a new file called [`main.py`](http://main.py) and insert the following code:

    ```python
    import requests
    import json

    url = "https://go.getblock.us/<ACCESS_TOKEN>"

    payload = json.dumps({
      "jsonrpc": "2.0",
      "method": "eth_blockNumber",
      "params": [],
      "id": "getblock.io"
    })
    headers = {
      'Content-Type': 'application/json'
    }

    response = requests.request("POST", url, headers=headers, data=payload)

    print(response.text)

    ```

    > Replace `<ACCESS_TOKEN>` with your actual access token from GetBlock.
5.  Run the script:

    ```bash
    python main.py
    ```

### Support & Resources

* [Hyperliquid Documentation](https://hyperliquid.gitbook.io/hyperliquid-docs/)
* [HyperEVM Developer Docs](https://hyperliquid.gitbook.io/hyperliquid-docs/for-developers/hyperevm)
* [Block Explorer](https://purrsec.com/)
