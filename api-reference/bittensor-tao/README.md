---
description: >-
  GetBlock provides fast and reliable access to Bittensor nodes via JSON-RPC
  API. Connect to the Bittensor network without running your own infrastructure.
hidden: true
---

# Bittensor(TAO)

Bittensor is a decentralized machine intelligence network — a blockchain-coordinated marketplace for AI services where miners produce digital commodities (model inference, embeddings, training contributions), validators evaluate that work, subnet creators define incentive mechanisms, and TAO holders stake to support validators. Launched in 2021, the network runs on its own blockchain, **Subtensor**, built on the Substrate framework (the same technology that powers Polkadot). As of 2026, the ecosystem has grown to **129+ active subnets** spanning AI agents, compute optimization, financial forecasting, and biotech use cases — up from 32 at the start of 2025.

The network underwent two pivotal changes in 2025: the **February 2025 Dynamic TAO (dTAO) upgrade** introduced market-driven emissions and made subnets directly investable for the first time; and the December 2025 change **halved daily TAO issuance** from 7,200 to 3,600 TAO per day, modeled on Bitcoin's supply schedule. Together, these shifts have shaped Bittensor into a maturing AI-economic primitive with real institutional adoption.

* #### Native Substrate JSON-RPC

This is the primary interface: `chain_*`, `state_*`, `system_*`, `author_*`, `payment_*`, plus the chain-specific custom namespaces `subnetInfo_*`, `neuronInfo_*`, `delegateInfo_*`, and `swap_*`. This is the canonical surface for reading subnet state, querying the metagraph, submitting extrinsics, and interacting with the TAO staking layer.

* #### Subtensor EVM JSON-RPC

This is the Ethereum compatibility layer: standard `eth_*`, `net_*`, `web3_*` methods, with chain ID **945** (`0x3B1` — UTF-8 encoding for "Alpha"). Enables EVM smart contract deployment on Subtensor without code changes; runs Solidity contracts via the Subtensor EVM runtime.

### Key Features

* **Dual RPC Interface**: GetBlock exposes both native Substrate JSON-RPC and Subtensor EVM JSON-RPC as separate endpoints — choose interface per endpoint when creating your connection in the GetBlock dashboard
* **Native Substrate RPC**: full access to the standard Substrate method surface (`chain_*`, `state_*`, `system_*`, `author_*`, `payment_*`) for block data, runtime state, transaction submission, and fee queries
* **Bittensor-Specific Custom RPCs**: chain-specific namespaces — `subnetInfo_*`, `neuronInfo_*`, `delegateInfo_*`, `swap_*` — for reading subnet metadata, neuron registrations, delegate stakes, and dTAO swap simulation. Not available on generic Substrate chains
* **Subtensor EVM**: deploy any Ethereum-compatible smart contract on the Bittensor blockchain without code modification. Use ethers.js, viem, Hardhat, Foundry, MetaMask — all work unmodified
* **WebSocket Subscriptions**: real-time block, finalized head, storage change, and extrinsic notifications via the Substrate subscription protocol (`chain_subscribeNewHeads`, `state_subscribeStorage`, etc.)
* **dTAO Era**: post-February 2025, subnets have their own market-driven emissions and Alpha tokens, with TAO ↔ Alpha swap pools that can be simulated via `swap_*` methods before submitting extrinsics
* **Bitcoin-Style Issuance**: as of December 2025, daily TAO issuance is 3,600 TAO/day (halved from 7,200) — modeled on Bitcoin's supply schedule, providing predictable monetary policy
* **129+ Active Subnets**: from AI agents to biotech, each subnet defines its own incentive mechanism. The metagraph (`neuronInfo_*`) gives you the full picture of which miners are scoring well on which subnets
* **Two Wallet Systems on One Chain**: Bittensor wallets (created via btcli/Bittensor SDK using sr25519/ed25519) sign Substrate extrinsics for TAO transfers and staking; EVM wallets (MetaMask, secp256k1) sign smart contract transactions. The two cannot cross over — your coldkey cannot deploy contracts; your MetaMask cannot transfer TAO
* **Yuma Consensus**: the network's incentive mechanism for ranking validator-miner pairs and distributing emissions — the algorithmic backbone of the subnet marketplace

{% hint style="info" %}
_**TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC SPECIFICATIONS**_

_GetBlock's Bittensor API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specifications are maintained by:_

* _**Standard Substrate methods**: the_ [_Polkadot.js documentation_](https://polkadot.js.org/docs/substrate/rpc) _and the_ [_Substrate node JSON-RPC reference_](https://docs.substrate.io/reference/command-line-tools/subkey/)
* _**Bittensor-specific methods**: the_ [_Subtensor source code_](https://github.com/opentensor/subtensor) _and_ [_Bittensor documentation_](https://docs.learnbittensor.org/)
* _**Subtensor EVM methods**: standard Ethereum JSON-RPC as defined by the_ [_Ethereum.org RPC specification_](https://ethereum.org/en/developers/docs/apis/json-rpc/)
{% endhint %}

## Network Information

| Property              | Value                                                                              |
| --------------------- | ---------------------------------------------------------------------------------- |
| Network Name          | Bittensor Mainnet (Finney)                                                         |
| Native Token          | TAO                                                                                |
| Token Decimals        | 9 (Substrate side) — `rao` is the smallest unit, 10^9 rao = 1 TAO                  |
| EVM Decimals          | 18 (EVM side) — wei-style representation for EVM tooling compatibility             |
| EVM Chain ID          | 945 (`0x3B1`) — UTF-8 encoding for the "Alpha" character                           |
| Block Time            | \~12 seconds                                                                       |
| Consensus             | Yuma Consensus (validators score miner work; emissions distributed by performance) |
| Framework             | Substrate (Rust)                                                                   |
| Daily TAO Issuance    | 3,600 TAO/day (halved from 7,200 in December 2025)                                 |
| Block Explorer        | [taostats.io](https://taostats.io/)                                                |
| Active Subnets (2026) | 129+ and growing                                                                   |
| Mainnet Genesis       | 2021                                                                               |
| dTAO Upgrade          | February 2025                                                                      |

## Base URL

GetBlock provides separate endpoints for the Substrate native interface and the Subtensor EVM interface. When creating an endpoint in the GetBlock dashboard, you choose the API interface — Substrate JSON-RPC or EVM-compatible JSON-RPC.

{% tabs %}
{% tab title="Substrate Native JSON-RPC" %}
{% code title="New York, USA" overflow="wrap" %}
```bash
https://shared.us-east-1.getblock.io/<Access-token>
```
{% endcode %}
{% endtab %}

{% tab title="Subtensor EVM JSON-RPC" %}
{% code title="New York, USA" overflow="wrap" %}
```bash
https://shared.us-east-1.getblock.io/<Access-token>

# websocket
wss://shared.us-east-1.getblock.io/<Access-token>
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**The Substrate and EVM interfaces are exposed as separate endpoints.** Create one endpoint for each interface in your GetBlock dashboard. Tokens are not interchangeable between interfaces — calling `eth_chainId` on a Substrate endpoint will not work, and calling `chain_getBlock` on an EVM endpoint will not work.
{% endhint %}

## Supported Networks

| Network           | Substrate JSON-RPC | EVM JSON-RPC | WSS | New York |
| ----------------- | ------------------ | ------------ | --- | -------- |
| Bittensor Mainnet | ✅                  | ✅            | ✅   | ✅        |

## Quickstart

In this section, you will learn how to make your first call against either the Substrate or EVM interface using:

* **Axios** (JavaScript / Node.js)
* **Python** (Requests library)

For Substrate-native interactions, the [Polkadot.js API](https://polkadot.js.org/docs/api) is the canonical typed SDK. For EVM interactions, use [ethers.js](https://docs.ethers.org/) or [viem](https://viem.sh/) as you would with any Ethereum chain.

{% tabs %}
{% tab title="JavaScript (Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir bittensor-api-quickstart
cd bittensor-api-quickstart
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

{% code title="index.js — Substrate interface example" overflow="wrap" %}
```javascript
import axios from 'axios';

// Substrate native JSON-RPC: get the chain name
const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "system_chain",
    "params": [],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://shared.us-east-1.getblock.io/<Access-token>',
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

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock. Make sure the endpoint is created with the Substrate interface selected.
{% endstep %}

{% step %}
### Run the script

```bash
node index.js
```

Expected output:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "Bittensor"
}
```

This confirms your endpoint is connected to the Bittensor mainnet (Finney).
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% stepper %}
{% step %}
### Setup the project directory

```bash
mkdir bittensor-api-quickstart
cd bittensor-api-quickstart
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

{% code title="main.py — Substrate interface example" %}
```python
import requests
import json

url = "https://shared.us-east-1.getblock.io/<Access-token>/"

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
### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

## Available API Methods

Bittensor exposes \~129 methods across all namespaces when you query `rpc_methods`. This documentation covers the 46 most-used methods, organized into three tiers: standard Substrate, Bittensor-specific custom RPCs, and Subtensor EVM. For complete enumeration of all available methods on a given endpoint, call `rpc_methods` (documented below).

### Substrate — `rpc` Module (1 method)

| Method       | Description                                                            |
| ------------ | ---------------------------------------------------------------------- |
| rpc\_methods | List all available RPC methods exposed by the connected Subtensor node |

### Substrate — `system` Module (7 methods)

| Method                   | Description                                                                  |
| ------------------------ | ---------------------------------------------------------------------------- |
| system\_chain            | Returns the chain name (`Bittensor` for mainnet)                             |
| system\_chainType        | Returns the chain type (`Live`, `Development`, `Local`, `Custom`)            |
| system\_name             | Returns the node implementation name (typically `subtensor`)                 |
| system\_version          | Returns the node implementation version                                      |
| system\_health           | Returns node health — peer count, syncing status, should-have-peers flag     |
| system\_properties       | Returns chain properties — token decimals (9), ss58Format, tokenSymbol (TAO) |
| system\_accountNextIndex | Returns the next transaction nonce for a given account address               |

### Substrate — `chain` Module (7 methods)

| Method                         | Description                                                     |
| ------------------------------ | --------------------------------------------------------------- |
| chain\_getBlock                | Returns a block by block hash (full block including extrinsics) |
| chain\_getBlockHash            | Returns the block hash for a given block number                 |
| chain\_getHeader               | Returns a block header by block hash                            |
| chain\_getFinalizedHead        | Returns the hash of the latest finalized block                  |
| chain\_subscribeNewHeads       | (WSS) Subscribes to new block headers as they're produced       |
| chain\_subscribeFinalizedHeads | (WSS) Subscribes to new finalized block headers                 |
| chain\_unsubscribeAllHeads     | Cancels an active heads subscription                            |

### Substrate — `state` Module (5 methods)

| Method                   | Description                                                                                      |
| ------------------------ | ------------------------------------------------------------------------------------------------ |
| state\_getStorage        | Returns the storage value at a given key (hex-encoded storage key built from pallet + item hash) |
| state\_getMetadata       | Returns the runtime metadata (pallet definitions, extrinsic signatures, storage layout)          |
| state\_getRuntimeVersion | Returns the current runtime version — spec\_name, spec\_version, transaction\_version            |
| state\_call              | Executes a runtime API call (the bridge to access runtime APIs like `SubnetInfoRuntimeApi`)      |
| state\_subscribeStorage  | (WSS) Subscribes to changes at given storage keys                                                |

### Substrate — `author` Module (3 methods)

| Method                          | Description                                                           |
| ------------------------------- | --------------------------------------------------------------------- |
| author\_submitExtrinsic         | Submits a signed extrinsic — returns the extrinsic hash               |
| author\_submitAndWatchExtrinsic | (WSS) Submits and watches an extrinsic through inclusion/finalization |
| author\_pendingExtrinsics       | Returns all currently pending extrinsics in the transaction pool      |

### Substrate — `payment` Module (1 method)

| Method             | Description                                                      |
| ------------------ | ---------------------------------------------------------------- |
| payment\_queryInfo | Returns fee, weight, and class information for a given extrinsic |

### Bittensor-Specific — `subnetInfo` Module (4 methods)

| Method                           | Description                                                                 |
| -------------------------------- | --------------------------------------------------------------------------- |
| subnetInfo\_getSubnetsInfo       | Returns information for all active subnets — netUid, name, owner, params    |
| subnetInfo\_getSubnetInfo        | Returns information for a single subnet by netUid                           |
| subnetInfo\_getSubnetHyperparams | Returns the hyperparameters governing a subnet's behavior (rho, kappa, ...) |
| subnetInfo\_getLockCost          | Returns the current TAO lock cost required to register a new subnet         |

### Bittensor-Specific — `neuronInfo` Module (2 methods)

| Method                 | Description                                                                                |
| ---------------------- | ------------------------------------------------------------------------------------------ |
| neuronInfo\_getNeurons | Returns the full neuron list (metagraph) for a subnet — hotkey, coldkey, stake, rank, etc. |
| neuronInfo\_getNeuron  | Returns a single neuron's data by subnet netUid and neuron UID                             |

### Bittensor-Specific — `delegateInfo` Module (3 methods)

| Method                     | Description                                                                       |
| -------------------------- | --------------------------------------------------------------------------------- |
| delegateInfo\_getDelegates | Returns all delegates available for TAO staking — hotkey, total stake, take, etc. |
| delegateInfo\_getDelegate  | Returns information for a single delegate by their hotkey                         |
| delegateInfo\_getDelegated | Returns all delegations made by a given coldkey                                   |

### Subtensor EVM — `web3` Module (1 method)

| Method              | Description                                                        |
| ------------------- | ------------------------------------------------------------------ |
| web3\_clientVersion | Returns the EVM client software version (typically Frontier-based) |

### Subtensor EVM — `net` Module (1 method)

| Method       | Description                                                          |
| ------------ | -------------------------------------------------------------------- |
| net\_version | Returns the network ID — `"945"` for Bittensor mainnet Subtensor EVM |

### Subtensor EVM — `eth` Module (11 methods)

| Method                     | Description                                                         |
| -------------------------- | ------------------------------------------------------------------- |
| eth\_chainId               | Returns the EVM chain ID — `0x3b1` (945) for Bittensor mainnet      |
| eth\_blockNumber           | Returns the current block number on the Subtensor EVM               |
| eth\_getBalance            | Returns the TAO balance of an EVM address (18 decimals on EVM side) |
| eth\_getCode               | Returns the bytecode at a contract address                          |
| eth\_getTransactionCount   | Returns the transaction count (nonce) for an EVM address            |
| eth\_getBlockByNumber      | Returns block info by number (or `latest`, `finalized`)             |
| eth\_getTransactionReceipt | Returns the receipt of a confirmed transaction                      |
| eth\_call                  | Executes a read-only contract call                                  |
| eth\_estimateGas           | Estimates gas required for a transaction                            |
| eth\_sendRawTransaction    | Submits a signed EVM transaction (EIP-155, chain ID 945)            |
| eth\_getLogs               | Returns event logs matching filter criteria                         |

## Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Official Bittensor Documentation](https://docs.learnbittensor.org/)
* [Subtensor GitHub Repository](https://github.com/opentensor/subtensor)
* [Bittensor Whitepaper](https://bittensor.com/whitepaper)
* [TaoStats Block Explorer](https://taostats.io/)
* [GetBlock Bittensor Product Page](https://getblock.io/nodes/tao/)
* [GetBlock Blog — Bittensor RPC Expansion to Shared Nodes (April 28, 2026)](https://getblock.io/blog/getblock-expands-bittensor-rpc-access/)
* [Polkadot.js Substrate RPC Reference](https://polkadot.js.org/docs/substrate/rpc) (for the inherited Substrate method surface)
* [Polkadot.js API Documentation](https://polkadot.js.org/docs/api) (canonical typed SDK for Substrate chains)
* [Subtensor EVM Tutorials](https://docs.learnbittensor.org/evm-tutorials) (deploying Solidity contracts on Bittensor)
* [Standard Ethereum JSON-RPC Reference](https://ethereum.org/en/developers/docs/apis/json-rpc/) (for the EVM-compatible surface)
* [Bittensor SDK (Python)](https://github.com/opentensor/bittensor) (canonical Python SDK)
* [Subtensor Source Code — Custom RPC Implementation](https://github.com/opentensor/subtensor/tree/main/pallets) (authoritative source for `subnetInfo_*`, `neuronInfo_*`, `delegateInfo_*` definitions)
