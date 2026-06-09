---
description: >-
  GetBlock provides fast and reliable access to Nervos via JSON-RPC API. Connect
  to the Nervos network without running your own infrastructure.
---

# Nervos CKB

Nervos CKB (Common Knowledge Base) is the Layer 1 of the Nervos Network — a public, permissionless, proof-of-work blockchain that takes a fundamentally different approach to state and computation.&#x20;

While Ethereum uses an account-based model with on-chain smart contracts and Bitcoin uses UTXOs that hold only value, Nervos CKB introduces the **Cell model**: every piece of on-chain state lives in a typed cell that can hold arbitrary data, with smart contracts ("scripts") written in any language that compiles to RISC-V. Native PoW via the Eaglesong hash function with NC-MAX (an improved Nakamoto consensus) produces blocks every \~10 seconds.&#x20;

The native token is CKByte (CKB) — 8 decimals; the smallest unit is Shannon (1 CKB = 100,000,000 Shannon). The RPC surface is organized into modules: **Chain** for canonical chain data, **Pool** for the transaction pool, **Net** for P2P network info, **Stats** for chain statistics, **Indexer** for cell/transaction lookups (built into the node since v0.105.0), **Subscription** for real-time WebSocket events, and **Experiment** for advanced features like Nervos DAO calculations.

### Key Features

* **Cell Model**: Programmable containers that hold arbitrary state plus typed scripts — a generalization of the UTXO model that supports arbitrary on-chain state and rich smart contracts
* **RISC-V Smart Contracts**: Scripts run on a deterministic RISC-V VM (CKB-VM) — write contracts in C, Rust, JavaScript (via QuickJS), or any language with a RISC-V target
* **Layered Architecture**: CKB is the store of assets; computation can happen on Layer 2 (Godwoken, Axon) or in off-chain generators with on-chain verification — keeps L1 lean while enabling rich applications
* **Eaglesong PoW**: ASIC-friendly hash function specifically designed for CKB, providing strong proof-of-work security with predictable mining economics
* **NC-MAX Consensus**: Improved Nakamoto consensus with \~10-second block times and dynamic difficulty — faster than Bitcoin while preserving its security properties
* **Nervos DAO**: Native protocol-level mechanism for locking CKB to receive secondary issuance — counterbalances state-rent inflation and rewards long-term holders
* **State Rent Model**: CKB is also a "right" to occupy state — long-term storage requires holding proportional CKB, aligning incentives between users and miners
* **Built-in Indexer**: Cell and transaction indexing built into the node since v0.105.0 — no separate indexer service required
* **Native Token CKByte**: 8 decimals — smallest unit Shannon. Fixed maximum issuance of 33.6 billion via base issuance, plus secondary issuance that funds protocol economics
* **Bech32 Addresses**: Human-readable addresses prefixed `ckb1...` (mainnet) and `ckt1...` (testnet)
* **WebSocket Subscriptions**: Full-duplex subscribe/unsubscribe for new tip headers, pool transaction events, proposed transactions, rejected transactions, and reorgs

{% hint style="info" %}
_**TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC SPECIFICATION**_

_GetBlock's Nervos CKB API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for the CKB JSON-RPC interface — including module organization, request/response shapes, and deprecation policy — is maintained by the Nervos Foundation and CKB development team and published at_ [_github.com/nervosnetwork/ckb/blob/master/rpc/README.md_](https://github.com/nervosnetwork/ckb/blob/master/rpc/README.md)_. For Cell model semantics, RISC-V CKB-VM details, Eaglesong PoW, NC-MAX consensus, and the Nervos DAO, consult the_ [_official Nervos documentation_](https://docs.nervos.org/)_._
{% endhint %}

{% hint style="warning" %}
**SCOPE: NERVOS CKB LAYER 1 ONLY — GODWOKEN L2 IS A SEPARATE PRODUCT**

_This documentation covers **Nervos CKB Layer 1** — the native chain with snake\_case methods, the Cell model, and RISC-V scripts._

_Nervos also has **Godwoken**, an EVM-compatible Layer 2 built on top of CKB that exposes the standard Ethereum JSON-RPC surface (`eth_blockNumber`, `eth_getBalance`, etc.). Godwoken is a separate product with a different URL pattern, different chain ID, and different developer experience. If you need EVM-compatible Solidity deployment on Nervos, you want Godwoken — not this package._

_For Godwoken documentation, see_ [_docs.godwoken.io_](https://docs.godwoken.io/) _or the_ [_Godwoken GitHub_](https://github.com/godwokenrises/godwoken)_._
{% endhint %}

## Network Information

| Property          | Value                                                                                            |
| ----------------- | ------------------------------------------------------------------------------------------------ |
| Network Name      | Nervos CKB Mainnet (Mirana)                                                                      |
| Chain Identifier  | `"ckb"` (mainnet), `"ckb_testnet"` (testnet)                                                     |
| Native Currency   | CKByte (CKB)                                                                                     |
| Decimals          | 8 (1 CKB = 100,000,000 Shannon)                                                                  |
| Block Time        | \~10 seconds                                                                                     |
| Consensus         | NC-MAX (improved Nakamoto consensus over Eaglesong PoW)                                          |
| Smart Contract VM | CKB-VM (RISC-V)                                                                                  |
| Data Model        | Cell model (extended UTXO with arbitrary data + typed scripts)                                   |
| Address Format    | Bech32 (`ckb1...` mainnet, `ckt1...` testnet)                                                    |
| Mainnet Launch    | November 19, 2019                                                                                |
| Block Explorer    | [explorer.nervos.org](https://explorer.nervos.org/), [ckbexplorer.com](https://ckbexplorer.com/) |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="New York, USA" %}
```bash
https://go.getblock.us/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Singapore, Singapore" %}
```bash
https://go.getblock.asia/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

All Nervos CKB JSON-RPC methods are called by sending a `POST` request to the base URL with a JSON-RPC 2.0 body. For Subscription module methods (`subscribe` / `unsubscribe`), use WebSocket: `wss://go.getblock.io/<ACCESS-TOKEN>/`.

## Supported Networks

| Network          | JSON-RPC | WSS | CKB (WebSocket) | CKB (JSON-RPC)         | Godwoken | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ---------------- | -------- | --- | --------------- | ---------------------- | -------- | ------------------ | ------------- | -------------------- |
| Mainnet (Mirana) | ✅        | ✅   | ✅               | <p></p><p>✅</p><p></p> | ✅        | ✅                  | ✅             | ✅                    |

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
mkdir nervos-api-quickstart
cd nervos-api-quickstart
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
    "method": "get_tip_header",
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
    "id": "getblock.io",
    "result": {
        "compact_target": "0x1a08a97e",
        "dao": "0x...",
        "epoch": "0x...",
        "hash": "0x...",
        "number": "0x...",
        "nonce": "0x...",
        "parent_hash": "0x...",
        "proposals_hash": "0x000...",
        "timestamp": "0x18f3b8d...",
        "transactions_root": "0x...",
        "extra_hash": "0x000...",
        "version": "0x0"
    }
}
```

The presence of a populated `result` object confirms you are connected to a synced Nervos CKB mainnet node.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% stepper %}
{% step %}
### Setup the project directory

```bash
mkdir nervos-api-quickstart
cd nervos-api-quickstart
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
    "method": "get_tip_header",
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

Nervos CKB exposes \~40 methods organized across 7 modules. Method names use **snake\_case** (e.g. `get_tip_header`) — different from Ethereum's camelCase convention.

### Module Chain (22 methods)

Canonical chain queries — blocks, transactions, headers, epochs, consensus parameters, fee statistics, and cell lookups.

| Method                                   | Description                                                            |
| ---------------------------------------- | ---------------------------------------------------------------------- |
| get\_tip\_header                         | Returns the header of the tip block in the canonical chain             |
| get\_tip\_block\_number                  | Returns the number of the tip block in the canonical chain             |
| get\_current\_epoch                      | Returns the epoch with the highest number in the canonical chain       |
| get\_epoch\_by\_number                   | Returns the epoch in the canonical chain with the specified number     |
| get\_block\_hash                         | Returns the hash of a block in the canonical chain by block number     |
| get\_block                               | Returns the information about a block by hash                          |
| get\_block\_by\_number                   | Returns the information about a block by block number                  |
| get\_header                              | Returns the information about a block header by hash                   |
| get\_header\_by\_number                  | Returns the information about a block header by block number           |
| get\_block\_economic\_state              | Returns block rewards and other economic state of a finalized block    |
| get\_block\_median\_time                 | Returns the past median timestamp used for time-locked transactions    |
| get\_block\_filter                       | Returns the block filter data for a block (for light clients)          |
| get\_consensus                           | Returns chain consensus parameters                                     |
| get\_fork\_block                         | Returns the information about a block by hash (including fork blocks)  |
| get\_transaction                         | Returns the information about a transaction by hash                    |
| get\_transaction\_proof                  | Returns a Merkle proof that transactions were included in a block      |
| verify\_transaction\_proof               | Verifies a Merkle proof and returns transaction hashes                 |
| get\_transaction\_and\_witness\_proof    | Returns a Merkle proof of transactions plus their witnesses            |
| verify\_transaction\_and\_witness\_proof | Verifies a Merkle proof of transactions and witnesses                  |
| get\_live\_cell                          | Returns the live cell at the given out point                           |
| estimate\_cycles                         | Returns the estimated cycles needed to execute a transaction's scripts |
| get\_fee\_rate\_statistics               | Returns fee rate statistics over recent blocks                         |

### Module Pool (5 methods)

Transaction pool submission and inspection.

| Method                      | Description                                                                    |
| --------------------------- | ------------------------------------------------------------------------------ |
| send\_transaction           | Submits a new transaction to the tx pool                                       |
| tx\_pool\_info              | Returns the statistics of the tx pool — total count, size, fees                |
| tx\_pool\_ready             | Returns `true` once the tx pool has finished initialization                    |
| get\_pool\_tx\_detail\_info | Returns detailed status of a transaction in the pool by hash                   |
| get\_raw\_tx\_pool          | Returns all transactions in the pool grouped by state (pending, proposed, gap) |

### Module Net (4 methods)

P2P network info — local node identity, connected peers, sync state, banned addresses.

| Method                 | Description                                                            |
| ---------------------- | ---------------------------------------------------------------------- |
| local\_node\_info      | Returns information about the local node (node ID, version, addresses) |
| get\_peers             | Returns the connected peers                                            |
| sync\_state            | Returns chain synchronization state                                    |
| get\_banned\_addresses | Returns the addresses currently banned by the node                     |

### Module Stats (2 methods)

| Method                 | Description                                                                     |
| ---------------------- | ------------------------------------------------------------------------------- |
| get\_blockchain\_info  | Returns statistics about the chain — chain name, median time, epoch, difficulty |
| get\_deployments\_info | Returns the active soft fork deployments and their states                       |

### Module Indexer (4 methods) — built-in since v0.105.0

Cell and transaction indexing — query live cells by lock or type script, paginated transaction history.

| Method               | Description                                                                              |
| -------------------- | ---------------------------------------------------------------------------------------- |
| get\_indexer\_tip    | Returns the indexed tip — sequence at which the indexer is current                       |
| get\_cells           | Returns live cells matching a lock or type script filter (paginated)                     |
| get\_transactions    | Returns transactions matching a lock or type script filter (paginated)                   |
| get\_cells\_capacity | Returns the total capacity of live cells matching a filter — the canonical balance query |

### Module Subscription (2 methods) — WebSocket only

Real-time event subscriptions over WebSocket.

| Method      | Description                                                                                                                  |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------- |
| subscribe   | Subscribes to a topic — `new_tip_header`, `new_tip_block`, `new_transaction`, `proposed_transaction`, `rejected_transaction` |
| unsubscribe | Cancels an existing subscription by ID                                                                                       |

### Module Experiment (1 method)

| Method                            | Description                                                       |
| --------------------------------- | ----------------------------------------------------------------- |
| calculate\_dao\_maximum\_withdraw | Calculates the maximum CKB a Nervos DAO deposit cell can withdraw |

### Restricted Modules (not available on shared infrastructure)

The following modules exist in the CKB node software but are not exposed on shared RPC infrastructure such as GetBlock's standard endpoints. They require self-hosted nodes with explicit configuration:

* **Miner module**: `get_block_template`, `submit_block` — for miners running their own node
* **Alert module**: `send_alert` — for broadcasting network-wide alerts (requires alert signing key)
* **Debug module**: `jemalloc_profiling_dump`, `update_main_logger`, `set_extra_logger` — for node operators only
* **IntegrationTest module**: `truncate`, `generate_block`, `process_block_without_verify`, etc. — for test-net development only

These methods are not documented per-page in this package. If you need access, contact GetBlock about dedicated-node options.

## Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Official Nervos Documentation](https://docs.nervos.org/)
* [CKB JSON-RPC Specification (canonical)](https://github.com/nervosnetwork/ckb/blob/master/rpc/README.md)
* [Nervos Network Whitepaper](https://github.com/nervosnetwork/rfcs/blob/master/rfcs/0001-positioning/0001-positioning.md)
* [Cell Model RFC](https://github.com/nervosnetwork/rfcs/blob/master/rfcs/0002-ckb/0002-ckb.md)
* [Nervos DAO RFC](https://github.com/nervosnetwork/rfcs/blob/master/rfcs/0023-dao-deposit-withdraw/0023-dao-deposit-withdraw.md)
* [Eaglesong PoW RFC](https://github.com/nervosnetwork/rfcs/blob/master/rfcs/0010-eaglesong/0010-eaglesong.md)
* [CKB Block Explorer (official)](https://explorer.nervos.org/)
* [CKB Explorer (alternative)](https://ckbexplorer.com/)
* [CKB SDK JavaScript](https://github.com/nervosnetwork/ckb-sdk-js)
* [CKB SDK Java](https://github.com/nervosnetwork/ckb-sdk-java)
* [CKB SDK Rust](https://github.com/nervosnetwork/ckb-sdk-rust)
* [Godwoken (EVM Layer 2)](https://docs.godwoken.io/) — separate product, not covered here
* [CKB-VM (RISC-V VM)](https://github.com/nervosnetwork/ckb-vm)
