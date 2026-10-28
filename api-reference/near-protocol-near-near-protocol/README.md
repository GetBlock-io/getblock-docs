---
description: >-
  GetBlock provides fast and reliable access to NEAR Protocol nodes via JSON-RPC
  API. Connect to the NEAR Protocol network without running your own
  infrastructure.
---

# NEAR Protocol (NEAR) - NEAR Protocol

NEAR Protocol is a high-performance, sharded Layer-1 blockchain with human-readable account names (`example.near`) and a developer-friendly runtime (contracts in Rust and JavaScript).

It is **not** EVM-compatible: NEAR exposes its own JSON-RPC 2.0 API with NEAR-native methods (`query`, `block`, `chunk`, `tx`, `send_tx`, and others) rather than the Ethereum `eth_*` surface. The native token is NEAR, denominated in yoctoNEAR (1 NEAR = 10^24 yoctoNEAR). This reference documents the NEAR JSON-RPC interface, available over HTTP and WebSocket.

## Key Features

* **Native JSON-RPC**: A JSON-RPC 2.0 API with NEAR-specific methods — not `eth_*`; account and contract reads go through the `query` method with a `request_type`
* **Sharded Layer-1**: Blocks are split into per-shard chunks (Nightshade sharding) — many methods take a `shard_id` or return per-chunk data
* **Named Accounts**: Human-readable account IDs (`example.near`) alongside 64-character implicit (hex) accounts
* **yoctoNEAR**: Balances and gas are denominated in yoctoNEAR (1 NEAR = 10^24 yoctoNEAR — 24 decimals)
* **Finality Control**: Most read methods accept `finality` (`optimistic` / `final`) or an explicit `block_id` (height or hash)
* **Access Keys**: Accounts hold multiple access keys — full-access or function-call keys with allowances — queried over RPC

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical specification for the NEAR JSON-RPC interface is maintained by NEAR and published at_ [_docs.near.org/api/rpc_](https://docs.near.org/api/rpc/providers)_. Method availability can depend on the node's nearcore version — some methods were renamed from `EXPERIMENTAL_` prefixes in nearcore 2.14.0._
{% endhint %}

## Network Information

| Property        | Value                                              |
| --------------- | -------------------------------------------------- |
| Network Name    | NEAR Protocol Mainnet                              |
| Chain ID        | mainnet                                            |
| Native Currency | NEAR                                               |
| Denomination    | yoctoNEAR (1 NEAR = 10^24 yoctoNEAR — 24 decimals) |
| Account Model   | Named accounts (`example.near`) + implicit (hex)   |
| Consensus       | Nightshade (sharded Proof-of-Stake)                |
| Finality        | Deterministic (doomslug + final)                   |
| Block Explorer  | [nearblocks.io](https://nearblocks.io/)            |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
NEAR Protocol is served from the **Frankfurt** region only — use the Frankfurt base URL above. WebSocket is available at `wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`.
{% endhint %}

## Supported Networks

| Network | Chain ID | JSON-RPC | WSS | GraphQL | MEV protected (WebSocket) | MEV protected (JSON-RPC) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | -------- | -------- | --- | ------- | ------------------------- | ------------------------ | ------------------ | ------------- | -------------------- |
| Mainnet | mainnet  | ✅        | ✅   | ❌       | ❌                         | ❌                        | ✅                  | ❌             | ❌                    |
| Testnet | testnet  | ✅        | ✅   | ❌       | ❌                         | ❌                        | ✅                  | ❌             | ❌                    |

## Quickstart

In this section, you will learn how to make your first call with either:

* Axios
* Python

{% tabs %}
{% tab title="Javascript" %}
Before you begin, you must have already installed `npm` or `yarn` on your local machine. If not, check out [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [yarn](https://classic.yarnpkg.com/lang/en/docs/install).

1.  Set up your project using this command:

    For npm:

    ```bash
    mkdir near-api-quickstart
    cd near-api-quickstart
    npm init --yes
    ```

    Or yarn:

    ```bash
    mkdir near-api-quickstart
    cd near-api-quickstart
    yarn init --yes
    ```

This creates a project directory named `near-api-quickstart` and initialises a Node.js project within it.

2.  Install Axios using this command:\
    Using npm:

    ```bash
    npm install axios
    ```

    Using yarn:

    ```bash
    yarn add axios
    ```
3. Create a new file and name it `index.js`. This is where you will make your first call.
4. Set ES module `"type": "module"` in your `package.json`.
5.  Add the following code to the file (`index.js`):

    ```js
    import axios from "axios"
    let data = JSON.stringify({
      "jsonrpc": "2.0",
      "method": "gas_price",
      "params": [
        169528908
      ],
      "id": "getblock.io"
    });

    let config = {
      method: 'post',
      maxBodyLength: Infinity,
      url: 'https://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>',
      headers: { 
        'Content-Type': 'application/json'
      },
      data : data
    };

    axios.request(config)
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

    The current gas price will log in the console like this:

    ```json
     {
        "jsonrpc": "2.0",
        "result": {
            "gas_price": "100000000"
        },
        "id": "getblock.io"
    }
    ```
{% endtab %}

{% tab title="Python" %}
Before you begin, you must have installed Python and Pip on your local machine.

1.  Set up your project using this command:

    ```bash
    mkdir near-api-quickstart
    cd near-api-quickstart
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

    url = "https://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>"

    payload = json.dumps({
      "jsonrpc": "2.0",
      "method": "gas_price",
      "params": [
        169528908
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
{% endtab %}
{% endtabs %}

## Available API Methods

### Network & Node

| Method                         | Description                                                                                                                                |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| status                         | Returns the node's general status: sync status, nearcore and protocol version, latest block hash and height, and the current validator set |
| network\_info                  | Returns the node's current network connections: active peers, known producers, and sent/received data counts                               |
| validators                     | Returns the active validator set and validation state for an epoch                                                                         |
| gas\_price                     | Returns the gas price for a specific block, or the latest price when called with \[null]                                                   |
| genesis\_config                | Returns the network's genesis configuration: the parameters the chain was launched with                                                    |
| EXPERIMENTAL\_protocol\_config | Returns the protocol configuration for the latest or a specific block: runtime, gas, and economic parameters currently in force            |

### Blocks & Chunks

| Method         | Description                                                                                                              |
| -------------- | ------------------------------------------------------------------------------------------------------------------------ |
| block          | Returns detailed information about a block, selected by finality ("final"/"optimistic") or by block\_id (height or hash) |
| chunk          | Returns the details of a chunk, by chunk hash, or by block\_id plus shard\_id                                            |
| block\_effects | Returns the state changes that occurred in a block — account, access-key, data, and contract-code changes                |

### Accounts & Contracts

| Method                | Description                                                                                                                                |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| query: view\_account  | Returns the basic account information for a NEAR account: balance, locked stake, storage usage, and the hash of its contract code (if any) |
| query: view\_code     | Returns the deployed contract code (WASM, base64-encoded) for an account, with its hash                                                    |
| query: view\_state    | Returns the key-value state stored under a contract account, optionally filtered by a base64 key prefix                                    |
| query: call\_function | Executes a read-only contract view method and returns its result, without creating a transaction                                           |

### Access Keys

| Method                         | Description                                                                                                                       |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| query: view\_access\_key       | Returns information about a single access key on an account: its nonce and permission (full access or a function-call permission) |
| query: view\_access\_key\_list | Returns all access keys registered to an account, each with its public key, nonce, and permission                                 |

### State Changes

| Method  | Description                                                    |
| ------- | -------------------------------------------------------------- |
| changes | Returns fine-grained state changes filtered by a changes\_type |

### Transactions

| Method                | Description                                                                                                                                    |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| send\_tx              | Broadcasts a signed transaction and waits for the requested execution milestone (wait\_until: NONE, INCLUDED, EXECUTED\_OPTIMISTIC, FINAL, etc |
| broadcast\_tx\_async  | Broadcasts a signed transaction and returns its hash immediately, without waiting for execution                                                |
| broadcast\_tx\_commit | Broadcasts a signed transaction and waits until it is included and executed, returning the full outcome                                        |
| tx                    | Returns the status of a previously submitted transaction, by tx\_hash plus sender\_account\_id (or by signed\_tx\_base64)                      |
| tx\_status            | Returns the transaction status plus the full list of receipts                                                                                  |
| EXPERIMENTAL\_receipt | Fetches a single receipt by its id, returning its predecessor, receiver, and action or data payload                                            |

### Maintenance

| Method                             | Description                                                                                                                                                                                  |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| EXPERIMENTAL\_maintenance\_windows | Returns the upcoming maintenance windows (block-height ranges) for a validator account — the periods in the current epoch when it is neither producing nor validating and can safely restart |

### Light Client

| Method                             | Description                                                                                                                                              |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| next\_light\_client\_block         | Returns the next light-client block after the given known block hash — the data a light client needs to advance and verify the chain without a full node |
| EXPERIMENTAL\_light\_client\_proof | Returns an execution-outcome inclusion proof (for a transaction or receipt) against a given light-client head, so a light client can verify the outcome  |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [NEAR RPC Documentation](https://docs.near.org/api/rpc/providers)
* [NEAR Account Model](https://docs.near.org/protocol/account-model)
* [NEAR Explorer (NearBlocks)](https://nearblocks.io/)
* [NEAR Website](https://near.org/)
