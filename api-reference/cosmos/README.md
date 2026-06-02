---
description: >-
  GetBlock provides fast and reliable access to Cosmos nodes via JSON-RPC API.
  Connect to the Cosmos network without running your own infrastructure.
---

# Cosmos

Cosmos Hub is the flagship blockchain of the Cosmos ecosystem — the "Internet of Blockchains" — and the original implementation of the Cosmos SDK and CometBFT (formerly Tendermint Core) consensus engine.&#x20;

Launched in March 2019 by the Interchain Foundation following the Cosmos whitepaper, the network pioneered application-specific Layer 1 design and the Inter-Blockchain Communication protocol (IBC), which today connects 100+ sovereign chains.&#x20;

The Hub itself secures Interchain Security (replicated security for consumer chains), hosts the ATOM token, and serves as the economic and governance center of the broader Cosmos network. Under the hood, Cosmos Hub runs the CometBFT consensus engine — a deterministic, Byzantine fault-tolerant state-machine replication engine — which exposes a well-defined RPC surface that this documentation covers.

### Key Features

* **CometBFT Consensus**: Deterministic instant finality (one block = final) via Tendermint-style BFT consensus — no fork choice, no probabilistic confirmation windows
* **Cosmos SDK Architecture**: Modular blockchain framework built around specialized modules (`bank`, `staking`, `gov`, `distribution`, `slashing`, `ibc`)
* **Inter-Blockchain Communication (IBC)**: First-class native interoperability with 100+ Cosmos ecosystem chains
* **Interchain Security**: The Hub leases its validator set to consumer chains, providing replicated security as a service
* **ATOM Token**: Native staking, governance, and IBC fee asset (uatom — 6 decimals)
* **Bech32 Addresses**: Human-readable `cosmos1...` addresses, with chain-specific prefixes for IBC-connected chains
* **PoS Validator Set**: \~180 active validators secure the network with delegated proof-of-stake
* **6-Second Block Time**: Predictable \~6s block production with single-slot finality
* **Liquid Staking**: Native support for liquid-staked ATOM derivatives (stATOM, stkATOM) via IBC-connected liquid staking chains
* **Governance On-Chain**: ATOM holders vote on protocol upgrades, parameter changes, and community spend proposals

{% hint style="info" %}
_**TECHNICAL DISCLAIMER: AUTHORITATIVE COMETBFT RPC SPECIFICATION**_

_GetBlock’s Cosmos Hub API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for the CometBFT RPC interface is maintained by the CometBFT team and published at_ [_docs.cometbft.com/main/spec/rpc_](https://docs.cometbft.com/main/spec/rpc/)_, with upstream source at_ [_github.com/cometbft/cometbft_](https://github.com/cometbft/cometbft)_. For Cosmos SDK protocol details, module specifications, and the IBC protocol, consult the_ [_official Cosmos documentation_](https://docs.cosmos.network/)_._
{% endhint %}

{% hint style="warning" %}
**SCOPE: COMETBFT RPC ONLY — REST / LCD IS A SEPARATE PRODUCT**

GetBlock exposes three distinct APIs for Cosmos Hub:&#x20;

1. **CometBFT JSON-RPC** (this documentation)
2. **Cosmos REST / LCD** (HTTP GET to paths like `/cosmos/bank/v1beta1/balances/{address}`),
3. **CometBFT WebSocket** (for event subscriptions).

**This package documents only the CometBFT RPC and WebSocket.** _The Cosmos REST API is a separate, very large surface auto-generated from each Cosmos SDK module's gRPC services (`cosmos.bank`, `cosmos.staking`, `cosmos.gov`, `cosmos.distribution`, etc.) and deserves its own dedicated documentation package. To browse REST endpoints today, use the_ [_official Cosmos REST API reference_](https://docs.cosmos.network/api)_._
{% endhint %}

## Network Information

| Property          | Value                                                                                       |
| ----------------- | ------------------------------------------------------------------------------------------- |
| Network Name      | Cosmos Hub Mainnet                                                                          |
| Chain ID          | `cosmoshub-4`                                                                               |
| Native Currency   | ATOM                                                                                        |
| Denomination      | `uatom` (1 ATOM = 1,000,000 uatom — 6 decimals)                                             |
| Block Time        | \~6 seconds                                                                                 |
| Consensus         | CometBFT (Byzantine fault-tolerant; instant finality)                                       |
| Active Validators | \~180                                                                                       |
| Address Format    | Bech32 `cosmos1...` (validator: `cosmosvaloper1...`)                                        |
| Block Explorer    | [mintscan.io/cosmos](https://www.mintscan.io/cosmos), [atomscan.com](https://atomscan.com/) |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

CometBFT supports **three transports** for the same methods:

* **JSON-RPC over HTTP (POST)** — sends a JSON-RPC 2.0 body to the base URL with the `method` field selecting the operation. **Default in this documentation.**
* **URI over HTTP (GET)** — appends the method name as a path segment (e.g. `/status`, `/block?height=12345`). Useful for browser debugging and one-off cURL calls. Most pages include a GET variant in the cURL tab where applicable.
* **JSON-RPC over WebSocket** — for real-time event subscriptions at `wss://go.getblock.io/<ACCESS-TOKEN>/websocket`. Used for `subscribe` / `unsubscribe` / `unsubscribe_all`.

## Supported Networks

| Network | JSON-RPC | WSS | URI (GET) | Frankfurt, Germany |
| ------- | -------- | --- | --------- | ------------------ |
| Mainnet | ✅        | ✅   | ✅         | ✅                  |

## Quickstart

In this section, you will learn how to make your first call with either:

* **Axios** (JavaScript / Node.js)
* **Python** (Requests library)

Before you begin, you must have already installed [`npm`](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [`yarn`](https://classic.yarnpkg.com/lang/en/docs/install) on your local machine (for the Axios example) or Python and pip (for the Python example).

{% tabs %}
{% tab title="JavaScript (Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir cosmos-api-quickstart
cd cosmos-api-quickstart
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

{% code title="index.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "status",
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
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual GetBlock access token.
{% endstep %}

{% step %}
### Run the script

```bash
node index.js
```

Expected output (truncated):

```json
{
    "jsonrpc": "2.0",
    "id": -1,
    "result": {
        "node_info": {
            "protocol_version": {
                "p2p": "8",
                "block": "11",
                "app": "0"
            },
            "id": "d22eecb26c324945056c1528f558580a3213937d",
            "listen_addr": "tcp://0.0.0.0:26656",
            "network": "cosmoshub-4",
            "version": "0.38.19",
            "channels": "40202122233038606100",
            "moniker": "9f5085e21705",
            "other": {
                "tx_index": "on",
                "rpc_address": "tcp://0.0.0.0:26657"
            }
        },
        "sync_info": {
            "latest_block_hash": "613E4F90A9B1772F527B94D25FC84E33D91B913A3DA38D247CE8A36710EA0257",
            "latest_app_hash": "F8841DAC63C6987DCED0B79B992134BF79DD15B47FC191DB75BB65A82657ACE9",
            "latest_block_height": "31378844",
            "latest_block_time": "2026-06-01T10:31:22.184767269Z",
            "earliest_block_hash": "4762CB4F2BFC2462911D0777D160D25CBF426343C831DFA26F35A31B371D1804",
            "earliest_app_hash": "807931A4854E57D9EB859695AD49D8DAD11B1D81A037E015F4ECEA168B53C3EC",
            "earliest_block_height": "26165371",
            "earliest_block_time": "2025-06-13T21:29:05.414994118Z",
            "catching_up": false
        },
        "validator_info": {
            "address": "8BFAB70EC8F05E6B106BEBD3F3F7F9F3797CD35C",
            "pub_key": {
                "type": "tendermint/PubKeyEd25519",
                "value": "f1m/S2ZdJmgDXX1YJWXmd+q28EPkYKu5OAL3zayoGsY="
            },
            "voting_power": "0"
        }
    }
}
```

`network: "cosmoshub-4"` and `catching_up: false` confirm you are connected to a synced Cosmos Hub mainnet node.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% stepper %}
{% step %}
### Setup the project directory

```bash
mkdir cosmos-api-quickstart
cd cosmos-api-quickstart
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

{% code title="main.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "status",
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
### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

## Available API Methods

CometBFT exposes \~24 RPC methods covering node info, block/chain data, transaction handling, mempool, ABCI queries, and event subscriptions.

### Node Info & Health

| Method    | Description                                                               |
| --------- | ------------------------------------------------------------------------- |
| health    | Returns empty result if the node is healthy; otherwise an error           |
| status    | Returns node info, pubkey, latest block hash, app hash, block height/time |
| net\_info | Returns network info (peers, listeners, p2p stats)                        |

### Blocks & Chain

| Method           | Description                                        |
| ---------------- | -------------------------------------------------- |
| blockchain       | Returns block metadata for a range of blocks       |
| block            | Returns block at a specific height                 |
| block\_by\_hash  | Returns a block by its hash                        |
| block\_results   | Returns the results of all transactions in a block |
| commit           | Returns the commit (signatures) for a block        |
| header           | Returns a block header at a specific height        |
| header\_by\_hash | Returns a block header by its hash                 |

### Consensus & Validators

| Method                 | Description                                                               |
| ---------------------- | ------------------------------------------------------------------------- |
| validators             | Returns the validator set at a specific height                            |
| consensus\_state       | Returns the current consensus state                                       |
| dump\_consensus\_state | Returns the full consensus state (validators, votes, peers)               |
| consensus\_params      | Returns consensus parameters at a specific height                         |
| genesis                | Returns the genesis document (small chains only)                          |
| genesis\_chunked       | Returns the genesis document in chunks (for large chains like Cosmos Hub) |

### Transactions

| Method                | Description                                                         |
| --------------------- | ------------------------------------------------------------------- |
| tx                    | Returns a transaction by hash, with optional inclusion proof        |
| tx\_search            | Searches transactions by event-attribute query                      |
| block\_search         | Searches blocks by event-attribute query                            |
| broadcast\_tx\_sync   | Broadcasts a transaction and returns after CheckTx (synchronous)    |
| broadcast\_tx\_async  | Broadcasts a transaction and returns immediately (asynchronous)     |
| broadcast\_tx\_commit | Broadcasts a transaction and waits until it is committed in a block |
| check\_tx             | Runs a CheckTx against the application without broadcasting         |

### Mempool

| Method                | Description                                               |
| --------------------- | --------------------------------------------------------- |
| unconfirmed\_txs      | Returns transactions in the mempool                       |
| num\_unconfirmed\_txs | Returns the count and size of transactions in the mempool |

### ABCI Application Queries

| Method      | Description                                                                |
| ----------- | -------------------------------------------------------------------------- |
| abci\_info  | Returns information about the ABCI application                             |
| abci\_query | Runs an ABCI query against the application (used by Cosmos SDK gRPC layer) |

### WebSocket Subscriptions

| Method           | Description                                            |
| ---------------- | ------------------------------------------------------ |
| subscribe        | Subscribes to events matching a Tendermint event query |
| unsubscribe      | Cancels a specific subscription                        |
| unsubscribe\_all | Cancels all subscriptions on the connection            |

## Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Official Cosmos Documentation](https://docs.cosmos.network/)
* [CometBFT RPC Reference](https://docs.cometbft.com/main/spec/rpc/)
* [Cosmos REST API Reference](https://docs.cosmos.network/api) — separate API surface, not covered in this package
* [Mintscan Explorer](https://www.mintscan.io/cosmos)
* [Atomscan Explorer](https://atomscan.com/)
* [Cosmos Chain Registry](https://github.com/cosmos/chain-registry)
* [CosmJS (JavaScript SDK)](https://github.com/cosmos/cosmjs)
* [cosmpy (Python SDK)](https://github.com/fetchai/cosmpy)
* [Cosmos SDK Module Specifications](https://docs.cosmos.network/main/build/modules)
* [IBC Protocol Documentation](https://ibc.cosmos.network/)
