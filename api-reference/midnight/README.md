---
description: >-
  Midnight API Reference for efficient interaction with opBNB nodes, enabling
  scalable, low-cost transactions and fast finality through Layer 2 solutions on
  the BSC blockchain.
---

# Midnight

Midnight is a data protection-focused blockchain developed by Input Output Global (IOG) — the team behind Cardano — designed to bring confidential smart contracts and zero-knowledge (ZK) cryptography to Web3. Operating as a partner chain to Cardano and built on the Substrate framework, Midnight enables developers to write privacy-preserving decentralized applications in TypeScript using the Compact language, which compiles directly to ZK circuits.&#x20;

The network uses a dual-token model:

* NIGHT for governance and staking, and the shielded token&#x20;
* DUST for transaction fees — to allow selective disclosure of sensitive information while maintaining the auditability and security of public blockchains.&#x20;

Midnight is currently in its testnet-02 phase, providing a stable sandbox for production-grade dApps that require granular data protection and regulatory compliance (such as GDPR).

## Key Features

* **Confidential Smart Contracts**: Smart contracts authored in TypeScript with the Compact ZK-DSL, compiling directly to zero-knowledge circuits
* **Zero-Knowledge Proofs (ZKPs)**: Native ZK primitives for verifying computation without revealing underlying data
* **Privacy-First Architecture**: Built for data protection and regulatory compliance, including GDPR-friendly selective disclosure
* **Dual-Token Economy**: NIGHT (unshielded governance/staking token) and DUST (shielded gas token derived from NIGHT)
* **Cardano Partner Chain**: Interoperable with Cardano; Cardano Stake Pool Operators can become Midnight Block Producers
* **Substrate-Based**: Built on the Substrate framework, exposing the standard Substrate JSON-RPC surface plus Midnight extensions
* **GRANDPA Finality**: Deterministic block finality via the Substrate GRANDPA finality gadget
* **Selective Disclosure**: Users choose what information to share and with whom, enabled by ZK selective-disclosure primitives
* **ZSwap Privacy Layer**: A native shielded UTXO model for confidential token transfers
* **WebSocket-First**: Full subscription support for real-time block, event, and storage updates via standard Substrate subscriptions

{% hint style="info" %}
_**TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC SPECIFICATION**_

_GetBlock’s Midnight API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical specification for Midnight's JSON-RPC interface is exposed live by every Midnight node via the `rpc_methods` call, and the Substrate JSON-RPC base is documented at the_ [_Polkadot SDK reference_](https://paritytech.github.io/polkadot-sdk/master/sc_rpc_api/)_. For protocol-level details, consensus parameters, and the dual-token model, consult the official Midnight documentation at_ [_docs.midnight.network_](https://docs.midnight.network/)_._
{% endhint %}

## Network Information

| Property             | Value                                                                       |
| -------------------- | --------------------------------------------------------------------------- |
| Network Name         | Midnight testnet-02                                                         |
| Stage                | Testnet (mainnet not yet launched)                                          |
| Native Tokens        | NIGHT (governance/staking), DUST (shielded gas, 6 decimals)                 |
| Block Time           | \~6 seconds (Substrate default)                                             |
| Consensus            | Custom privacy-aware Proof-of-Stake (Aura authoring + GRANDPA finality)     |
| Architecture         | Substrate partner chain to Cardano                                          |
| Smart Contract Layer | Compact (ZK-DSL compiling to circuits); TypeScript SDK                      |
| EVM Compatible       | No                                                                          |
| Address Format       | SS58 (Substrate-style)                                                      |
| Block Explorer       | [Midnight Explorer (community)](https://midnight-explorer-sand.vercel.app/) |

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

{% hint style="info" %}
All Midnight JSON-RPC methods are called by sending a `POST` request to the base URL with a standard JSON-RPC 2.0 body. For real-time subscriptions (new blocks, finalized heads, runtime events) use the WebSocket scheme: `wss://go.getblock.io/<ACCESS-TOKEN>/`.
{% endhint %}

## Supported Networks

| Network    | JSON-RPC | WSS | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ---------- | -------- | --- | ------------------ | ------------- | -------------------- |
| testnet-02 | ✅        | ✅   | ✅                  | ✅             | ✅                    |

Midnight mainnet has not yet been launched. This documentation targets **testnet-02**, the current stable development network.

## Quickstart

In this section, you will learn how to make your first call with either:

* **Axios** (JavaScript / Node.js)
* **Python** (Requests library)

Before you begin, you must have already installed [`npm`](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [`yarn`](https://classic.yarnpkg.com/lang/en/docs/install) on your local machine (for the Axios example) or Python and pip (for the Python example).

{% tabs %}
{% tab title="JavaScript (Axios)" %}
{% stepper %}
{% step %}
#### Setup project

Create and initialize a new project:

```bash
mkdir midnight-api-quickstart
cd midnight-api-quickstart
npm init --yes
```
{% endstep %}

{% step %}
#### Install Axios

```bash
npm install axios
```
{% endstep %}

{% step %}
#### Create file

Create a new file named `index.js`. This is where you will make your first call.
{% endstep %}

{% step %}
#### Set ES module type

Set the ES module `"type": "module"` in your `package.json`.
{% endstep %}

{% step %}
#### Add code

Add the following code to `index.js`:

{% code title="index.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "system_chain",
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
#### Run the script

```bash
node index.js
```

Expected output (example):

```json
{
    "jsonrpc": "2.0",
    "result": "Midnight Preprod",
    "id": "getblock.io"
}
```
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% stepper %}
{% step %}
#### Setup the project directory

```bash
mkdir midnight-api-quickstart
cd midnight-api-quickstart
```
{% endstep %}

{% step %}
#### Create and activate a virtual environment

```bash
python -m venv venv
source venv/bin/activate

# On Windows, use venv\Scripts\activate
```
{% endstep %}

{% step %}
#### Install requests

```bash
pip install requests
```
{% endstep %}

{% step %}
#### Create script

Create a file called `main.py` with the following content:

{% code title="main.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "system_chain",
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
#### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

## Available API Methods

Midnight exposes the full Substrate JSON-RPC surface, plus Midnight-specific extensions and partner chain primitives. Every method is callable via HTTP POST or over a WebSocket connection. Subscription-style methods are only available over WSS.

### System Information & Networking

| Method             | Description                                                     |
| ------------------ | --------------------------------------------------------------- |
| system\_chain      | Returns the name of the connected chain                         |
| system\_chainType  | Returns the chain type (`Live`, `Development`, `Local`)         |
| system\_health     | Returns node health and sync status                             |
| system\_properties | Returns chain properties (SS58 format, token decimals, symbols) |
| system\_syncState  | Returns sync status and highest known block                     |
| system\_version    | Returns the node implementation version                         |
| system\_name       | Returns the node implementation name                            |

### Accounts & Keys

| Method                   | Description                                           |
| ------------------------ | ----------------------------------------------------- |
| account\_nextIndex       | Returns the next nonce (account index) for an account |
| system\_accountNextIndex | Alias to `account_nextIndex`                          |

### GRANDPA Finality

| Method                 | Description                                                     |
| ---------------------- | --------------------------------------------------------------- |
| grandpa\_proveFinality | Provides a justification proof for finalized blocks via GRANDPA |
| grandpa\_roundState    | Returns the current GRANDPA round state                         |

### Off-chain Worker Storage

| Method                    | Description                                     |
| ------------------------- | ----------------------------------------------- |
| offchain\_localStorageGet | Reads data from local off-chain worker storage  |
| offchain\_localStorageSet | Writes data into local off-chain worker storage |

### Archive (Unstable v2 API)

| Method                             | Description                                 |
| ---------------------------------- | ------------------------------------------- |
| archive\_unstable\_body            | Fetches the body of a historical block      |
| archive\_unstable\_call            | Executes a runtime call at a specific block |
| archive\_unstable\_finalizedHeight | Returns the height of the finalized block   |
| archive\_unstable\_genesisHash     | Returns the genesis block hash              |
| archive\_unstable\_hashByHeight    | Returns the block hash at a given height    |
| archive\_unstable\_header          | Fetches the header of a historical block    |
| archive\_unstable\_storage         | Returns historical storage values           |

### Midnight-Specific

| Method                      | Description                                             |
| --------------------------- | ------------------------------------------------------- |
| midnight\_apiVersions       | Lists supported Midnight API versions on the node       |
| midnight\_contractState     | Fetches the on-chain state of a Midnight smart contract |
| midnight\_decodeEvents      | Decodes event data emitted by Midnight smart contracts  |
| midnight\_jsonBlock         | Returns a full block in human-readable JSON format      |
| midnight\_jsonContractState | Returns human-readable JSON-formatted contract state    |
| midnight\_zswapChainState   | Returns the ZSwap (shielded UTXO layer) state           |

### Sidechain & Partner Chain

| Method                          | Description                                                  |
| ------------------------------- | ------------------------------------------------------------ |
| sidechain\_getAriadneParameters | Returns Ariadne protocol parameters used for privacy         |
| sidechain\_getEpochCommittee    | Returns the validator committee for a given epoch            |
| sidechain\_getParams            | Returns current sidechain configuration parameters           |
| sidechain\_getStatus            | Returns current sidechain status (sync state and parameters) |

### Chain & Block Data

| Method                   | Description                                                                    |
| ------------------------ | ------------------------------------------------------------------------------ |
| chain\_getBlock          | Returns full block data by block hash                                          |
| chain\_getBlockHash      | Returns the block hash for a given block number                                |
| chain\_getFinalizedHead  | Returns the latest finalized block hash (`chain_getFinalisedHead` is an alias) |
| chain\_getHead           | Returns the current best block hash                                            |
| chain\_getHeader         | Returns the header of a given block                                            |
| chain\_getRuntimeVersion | Returns the current runtime version                                            |

### State & Storage

| Method                   | Description                                                           |
| ------------------------ | --------------------------------------------------------------------- |
| state\_call              | Executes a runtime call without submitting an extrinsic               |
| state\_callAt            | Executes a runtime call at a specific block                           |
| state\_getMetadata       | Returns the current runtime metadata                                  |
| state\_getRuntimeVersion | Returns the runtime version at the current best block                 |
| state\_getKeys           | Returns a list of storage keys (deprecated, use `state_getKeysPaged`) |
| state\_getKeysPaged      | Returns paged storage keys                                            |
| state\_getKeysPagedAt    | Returns paged storage keys at a specific block                        |
| state\_getStorage        | Returns a storage value for a given key                               |
| state\_getStorageAt      | Returns a storage value at a given block                              |
| state\_getStorageHash    | Returns the hash of a storage entry                                   |
| state\_getStorageSize    | Returns the byte size of a storage entry                              |
| state\_getReadProof      | Returns a Merkle proof of inclusion for storage entries               |
| state\_queryStorageAt    | Queries storage at a specific block                                   |

### Child State

| Method                        | Description                                    |
| ----------------------------- | ---------------------------------------------- |
| childstate\_getKeys           | Returns keys from child storage                |
| childstate\_getKeysPaged      | Returns paged keys from child storage          |
| childstate\_getStorage        | Returns a storage entry from child storage     |
| childstate\_getStorageEntries | Returns multiple entries from child storage    |
| childstate\_getStorageHash    | Returns the hash of a child storage value      |
| childstate\_getStorageSize    | Returns the byte size of a child storage value |

### Author (Transaction Submission)

| Method                    | Description                               |
| ------------------------- | ----------------------------------------- |
| author\_submitExtrinsic   | Submits a signed extrinsic to the network |
| author\_pendingExtrinsics | Returns extrinsics currently in the pool  |
| author\_removeExtrinsic   | Removes extrinsics from the pool          |

### RPC Meta

| Method       | Description                                                         |
| ------------ | ------------------------------------------------------------------- |
| rpc\_methods | Returns the list of all RPC methods supported by the connected node |

## Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Official Midnight Documentation](https://docs.midnight.network/)
* [Midnight Network Website](https://midnight.network/)
* [Midnight RPC Node Setup Guide](https://docs.midnight.network/nodes/rpc-node)
* [Midnight Block Producer Guide](https://midnight.network/blog/how-to-become-midnight-block-producer-testnet)
* [@polkadot/api Documentation](https://polkadot.js.org/docs/)
* [Compact Smart Contract Language](https://docs.midnight.network/develop/tutorial/building/compact-language)
* [Substrate JSON-RPC Reference](https://paritytech.github.io/polkadot-sdk/master/sc_rpc_api/)
* [Midnight Explorer (Community)](https://midnight-explorer-sand.vercel.app/)
