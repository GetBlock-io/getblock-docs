---
description: >-
  GetBlock provides fast and reliable access to Monad nodes via JSON-RPC API.
  Connect to the Monad network without running your own infrastructure.
---

# Monad (MON)

### Overview

Monad is a high-performance Layer 1 blockchain engineered for speed without sacrificing security or decentralization. It delivers up to 10,000 transactions per second (TPS), sub-second finality, and near-zero gas fees while maintaining full Ethereum Virtual Machine (EVM) compatibility at the bytecode level.

#### Key Features

* **Parallel Execution**: Optimistic parallel transaction processing for maximum throughput
* **Full EVM Compatibility**: Deploy Solidity contracts without modification
* **MonadBFT Consensus**: Custom Byzantine Fault Tolerant protocol with \~400ms block times
* **Sub-Second Finality**: Transaction finality in approximately 800ms
* **MonadDB**: Custom database optimized for blockchain state access
* **Asynchronous Execution**: Pipelined consensus and execution for higher throughput
* **10,000 TPS**: Industry-leading transaction throughput for EVM chains
* **Low Gas Fees**: Near-zero transaction costs

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._&#x20;

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and streamlined developer experience optimization. The canonical and normative specification for Ethereum Virtual Machine (EVM) JSON-RPC methods is solely maintained and published through the official Ethereum Foundation documentation portal at_ [_ethereum.org_](http://ethereum.org/)_. This resource constitutes the sole authoritative reference implementation of the JSON-RPC 2.0 protocol interface across EVM-compatible execution clients._
{% endhint %}

### Network Information

| Property          | Value                       |
| ----------------- | --------------------------- |
| Network Name      | Monad Mainnet               |
| Chain ID          | 143                         |
| Native Currency   | MON                         |
| Block Time        | \~400ms                     |
| Finality          | \~800ms                     |
| Max TPS           | 10,000                      |
| EVM Compatibility | Full bytecode compatibility |
| Consensus         | MonadBFT (Proof of Stake)   |

### Base URL

{% tabs %}
{% tab title="Franfurt, Germany" %}
```bash
https://go.getblock.io
```
{% endtab %}

{% tab title="New York, USA" %}
```bash
https://go.getblock.us
```
{% endtab %}

{% tab title="Singapore, Singapore" %}
```bash
https://go.getblock.asia
```
{% endtab %}
{% endtabs %}

### Supported Network

* Mainnet
* Testnet

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
mkdir monad-api-quickstart
cd monad-api-quickstart
npm init --yes
```
{% endtab %}

{% tab title="Yarn" %}
```bash
mkdir monad-api-quickstart
cd monad-api-quickstart
yarn init --yes
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
This creates a project directory named `monad-api-quickstart` and initialises a Node.js project within it.
{% endhint %}

2. Install Axios using this command:\
   Using npm:

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

{% hint style="info" %}
Replace `<ACCESS_TOKEN>` with your actual access token from GetBlock.
{% endhint %}

3.  Run the script:

    ```bash
    node index.js
    ```

    The sequence number and authentication key log in your console like this:

    ```json
    {
        "jsonrpc": "2.0",
        "id": "getblock.io",
        "result": "0x293ccbc"
    }
    ```

#### Quickstart with Python and Requests

Before you begin, you must have installed Python and Pip on your local machine.

1.  Set up your project using this command:

    ```bash
    mkdir bitcoin-api-quickstart
    cd bitcoin-api-quickstart
    ```
2.  Set up a virtual environment to isolate dependencies:

    ```bash
    python -m venv venv

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



{% hint style="info" %}
Replace `<ACCESS_TOKEN>` with your actual access token from GetBlock.
{% endhint %}

5. Run the script:

```bash
python main.py
```

### Available API Methods

GetBlock provides access to standard Ethereum JSON-RPC methods plus Monad-specific extensions:

#### Reading Data (State Methods)

| Method                   | Description                                    |
| ------------------------ | ---------------------------------------------- |
| eth\_blockNumber         | Returns the current block number               |
| eth\_getBalance          | Returns the balance of an address              |
| eth\_getStorageAt        | Returns the value at a storage position        |
| eth\_getTransactionCount | Returns the transaction count (nonce)          |
| eth\_getCode             | Returns the code at an address                 |
| eth\_call                | Executes a call without creating a transaction |
| eth\_getProof            | Returns account and storage proofs             |

#### Block Information

| Method                                | Description                               |
| ------------------------------------- | ----------------------------------------- |
| eth\_getBlockByHash                   | Returns block information by hash         |
| eth\_getBlockByNumber                 | Returns block information by number       |
| eth\_getBlockReceipts                 | Returns all receipts for a block          |
| eth\_getBlockTransactionCountByHash   | Returns transaction count by block hash   |
| eth\_getBlockTransactionCountByNumber | Returns transaction count by block number |

#### Transaction Methods

| Method                                   | Description                                                  |
| ---------------------------------------- | ------------------------------------------------------------ |
| eth\_getTransactionByHash                | Returns transaction by hash                                  |
| eth\_getTransactionByBlockHashAndIndex   | Returns transaction by block hash and index                  |
| eth\_getTransactionByBlockNumberAndIndex | Returns transaction by block number and index                |
| eth\_getTransactionReceipt               | Returns the receipt of a transaction                         |
| eth\_sendRawTransaction                  | Submits a signed transaction                                 |
| eth\_sendRawTransactionSync              | Submits transaction and waits for inclusion (Monad-specific) |

#### Gas and Fee Estimation

| Method                    | Description                              |
| ------------------------- | ---------------------------------------- |
| eth\_gasPrice             | Returns the current gas price            |
| eth\_maxPriorityFeePerGas | Returns suggested priority fee           |
| eth\_feeHistory           | Returns fee history for blocks           |
| eth\_estimateGas          | Estimates gas for a transaction          |
| eth\_createAccessList     | Creates an access list for a transaction |

#### Logs and Events

| Method                           | Description                           |
| -------------------------------- | ------------------------------------- |
| eth\_getLogs                     | Returns logs matching filter criteria |
| eth\_newFilter                   | Creates a new filter                  |
| eth\_newBlockFilter              | Creates a new block filter            |
| eth\_newPendingTransactionFilter | Creates pending transaction filter    |
| eth\_getFilterChanges            | Returns filter changes                |
| eth\_getFilterLogs               | Returns logs for a filter             |
| eth\_uninstallFilter             | Removes a filter                      |

#### Chain Information

| Method              | Description              |
| ------------------- | ------------------------ |
| eth\_chainId        | Returns the chain ID     |
| eth\_syncing        | Returns sync status      |
| net\_version        | Returns the network ID   |
| net\_listening      | Returns listening status |
| net\_peerCount      | Returns number of peers  |
| web3\_clientVersion | Returns client version   |

#### Debug Methods

| Method                    | Description                        |
| ------------------------- | ---------------------------------- |
| debug\_traceTransaction   | Returns transaction trace          |
| debug\_traceCall          | Traces a call                      |
| debug\_traceBlockByHash   | Traces all transactions in a block |
| debug\_traceBlockByNumber | Traces block by number             |

### Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

### See Also

* [Monad Developer Documentation](https://docs.monad.xyz/)
* [Monad Block Explorer - MonadVision](https://monadvision.com/)
* [Monad Block Explorer - Monadscan](https://monadscan.com/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/developers/docs/apis/json-rpc/)
