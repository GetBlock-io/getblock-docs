---
description: >-
  Bitcoin Network API Reference for secure and scalable interaction with BTC
  nodes, enabling peer-to-peer transactions and decentralized finance on the
  world’s first cryptocurrency block
---

# Bitcoin (BTC)

### Overview

Bitcoin is the first and largest decentralized cryptocurrency network, launched in 2009 by the pseudonymous Satoshi Nakamoto. It operates as a peer-to-peer electronic cash system, enabling trustless value transfer without intermediaries.

#### Key Features

* **Decentralized Network**: Secured by thousands of nodes worldwide with no central authority
* **Proof of Work Consensus**: Mining-based security with SHA-256 hashing algorithm
* **21 Million Supply Cap**: Fixed maximum supply creating digital scarcity
* **10-Minute Block Time**: Average block confirmation every 10 minutes
* **SegWit & Taproot**: Modern protocol upgrades for scalability and privacy
* **PSBT Support**: Partially Signed Bitcoin Transactions for secure multi-party signing
* **Lightning Network Compatible**: Layer 2 scaling solution support

## Quickstart

In this section, you will learn how to make your first call with either:

* Axios
* Python

### Quickstart with Axios

Before you begin, you must have already installed `npm` or `yarn` on your local machine. If not, check out [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [yarn](https://classic.yarnpkg.com/lang/en/docs/install).

1. Set up your project using this command:

{% tabs %}
{% tab title="npm" %}
```bash
mkdir bitcoin-api-quickstart
cd bitcoin-api-quickstart
npm init --yes
```
{% endtab %}

{% tab title="Yarn" %}
```bash
mkdir bitcoin-api-quickstart
cd bitcoin-api-quickstart
yarn init --yes
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
This creates a project directory named `bitcoin-api-quickstart` and initialises a Node.js project within it.
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
    import axios from 'axios'
    let data = JSON.stringify({
        "jsonrpc": "2.0",
        "method": "analyzepsbt",
        "params": [
            "cHNidP8BAEICAAAAATJIj43goxJCsLjdKM5Uly44S8Vp6cim1VW89Rx5fFPpAAAAAAD/////AQAAAAAAAAAABmoEAAECAwAAAAAAAAA="
        ],
        "id": "getblock.io"
    });

    let config = {
      method: "post",
      maxBodyLength: Infinity,
      url: "https://go.getblock.io/<ACCESS_TOKEN>",
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
        "result": {
            "inputs": [
                {
                    "has_utxo": false,
                    "is_final": false,
                    "next": "updater"
                }
            ],
            "next": "updater"
        },
        "id": "getblock.io"
    }
    ```

### Quickstart with Python and Requests

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

    url = "https://go.getblock.io/<ACCESS_TOKEN>"

    payload = json.dumps({
        "jsonrpc": "2.0",
        "method": "analyzepsbt",
        "params": [
            "cHNidP8BAEICAAAAATJIj43goxJCsLjdKM5Uly44S8Vp6cim1VW89Rx5fFPpAAAAAAD/////AQAAAAAAAAAABmoEAAECAwAAAAAAAAA="
        ],
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

### Base URL

{% tabs %}
{% tab title="Franfurt, Germany" %}
```bash
https://go.getblock.io
```
{% endtab %}
{% endtabs %}

### Supported Network

* Mainnet
* Testnet

### Available API Methods

GetBlock provides access to 45 Bitcoin RPC endpoints organized into the following categories:

#### PSBT & Transaction Building (13 methods)

Methods for creating, analyzing, and finalizing Partially Signed Bitcoin Transactions.

| Method                | Description                                         |
| --------------------- | --------------------------------------------------- |
| analyzepsbt           | Analyzes and provides information about a PSBT      |
| combinepsbt           | Combines multiple PSBTs into one                    |
| combinerawtransaction | Combines multiple partially signed raw transactions |
| converttopsbt         | Converts a raw transaction to PSBT format           |
| createpsbt            | Creates a new PSBT with given inputs and outputs    |
| createrawtransaction  | Creates a raw transaction                           |
| decodepsbt            | Decodes a PSBT to JSON                              |
| decoderawtransaction  | Decodes a raw transaction to JSON                   |
| decodescript          | Decodes a hex-encoded script                        |
| finalizepsbt          | Finalizes a PSBT for broadcast                      |
| fundrawtransaction    | Adds inputs to a raw transaction                    |
| joinpsbts             | Joins multiple PSBTs with different inputs          |
| utxoupdatepsbt        | Updates PSBT with UTXO data                         |

#### Blockchain & Block Information (13 methods)

Methods for querying blockchain state and block data.

| Method                | Description                             |
| --------------------- | --------------------------------------- |
| getbestblockhash      | Returns the hash of the best block      |
| getblock              | Returns block data for a given hash     |
| getblockchaininfo     | Returns blockchain state information    |
| getblockcount         | Returns the current block height        |
| getblockfilter        | Returns BIP 157 block filter            |
| getblockhash          | Returns block hash at given height      |
| getblockheader        | Returns block header information        |
| getblockstats         | Returns per-block statistics            |
| getblocktemplate      | Returns data for mining a block         |
| getchaintips          | Returns information about chain tips    |
| getchaintxstats       | Returns transaction statistics          |
| getdifficulty         | Returns the current mining difficulty   |
| getfinalizedblockhash | Returns the hash of the finalized block |

#### Mempool (6 methods)

Methods for querying the memory pool of unconfirmed transactions.

| Method                | Description                                  |
| --------------------- | -------------------------------------------- |
| getmempoolancestors   | Returns mempool ancestors of a transaction   |
| getmempooldescendants | Returns mempool descendants of a transaction |
| getmempoolentry       | Returns mempool data for a transaction       |
| getmempoolinfo        | Returns mempool state information            |
| getrawmempool         | Returns all transaction IDs in mempool       |
| testmempoolaccept     | Tests if transactions would be accepted      |

#### Mining & Network (2 methods)

Methods for mining information and network statistics.

| Method           | Description                         |
| ---------------- | ----------------------------------- |
| getmininginfo    | Returns mining-related information  |
| getnetworkhashps | Returns estimated network hash rate |

#### Transaction (4 methods)

Methods for querying and broadcasting transactions.

| Method             | Description                                    |
| ------------------ | ---------------------------------------------- |
| getrawtransaction  | Returns raw transaction data                   |
| gettxout           | Returns details about an unspent output        |
| gettxoutproof      | Returns proof that a transaction is in a block |
| sendrawtransaction | Broadcasts a raw transaction                   |

#### Utility (7 methods)

Utility methods for address derivation, validation, and node information.

| Method           | Description                           |
| ---------------- | ------------------------------------- |
| deriveaddresses  | Derives addresses from a descriptor   |
| estimatesmartfee | Estimates fee for confirmation target |
| getindexinfo     | Returns index status information      |
| getmemoryinfo    | Returns memory usage information      |
| getrpcinfo       | Returns RPC server information        |
| validateaddress  | Validates a Bitcoin address           |
| verifymessage    | Verifies a signed message             |

### Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

### See Also

* [Bitcoin Developer Documentation](https://developer.bitcoin.org/)
* [Bitcoin Core RPC Reference](https://developer.bitcoin.org/reference/rpc/)
* [BIP Repository](https://github.com/bitcoin/bips)
