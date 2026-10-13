---
description: >-
  GetBlock provides fast and reliable access to IOTA nodes via JSON-RPC API.
  Connect to the IOTA network without running your own infrastructure.
---

# IOTA

IOTA is a Layer-1 network built on a Move-based, object-centric protocol (the IOTA Rebased architecture, a fork of the Sui stack).&#x20;

State is modeled as typed objects owned by addresses, smart contracts are written in Move, and the native token is IOTA, denominated in NANOs (1 IOTA = 1,000,000,000 NANOs).&#x20;

GetBlock exposes the IOTA node JSON-RPC API: a JSON-RPC 2.0 interface over HTTP whose methods live in the `iota_` (core read/write and Move utilities), `iotax_` (coin, extended indexer, and governance queries), and `unsafe_` (server-side transaction building) namespaces.

## Key Features

* **Move + Objects**: State is typed Move objects owned by addresses, not account balances in a single trie
* **JSON-RPC 2.0**: A single POST endpoint; the method and positional `params` array select the call
* **Namespaced API**: `iota_` core methods, `iotax_` coin/indexer/governance queries, and `unsafe_` transaction builders
* **Checkpoints**: Chain progress is measured in checkpoints rather than blocks
* **NANOs Precision**: Balances and gas are in NANOs (1 IOTA = 1,000,000,000 NANOs)
* **Build-Sign-Submit**: The node builds unsigned `unsafe_` transactions; clients sign and broadcast with `iota_executeTransactionBlock`

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical specification is the IOTA node JSON-RPC API, published at_ [_docs.iota.org/iota-api-ref_](https://docs.iota.org/iota-api-ref)_; protocol and developer documentation is at_ [_docs.iota.org_](https://docs.iota.org/)_._
{% endhint %}

{% hint style="warning" %}
This GetBlock IOTA endpoint is served over **JSON-RPC (HTTP) only** — there is no WebSocket endpoint — so the subscription methods `iotax_subscribeEvent` and `iotax_subscribeTransaction` are not available and are not documented here. The API is available in all three regions.
{% endhint %}

## Network Information

| Property          | Value                               |
| ----------------- | ----------------------------------- |
| Network Name      | IOTA Mainnet                        |
| Protocol          | Move / object model (IOTA Rebased)  |
| Native Currency   | IOTA (1 IOTA = 1,000,000,000 NANOs) |
| Contract Language | Move                                |
| API               | JSON-RPC 2.0 over HTTP              |
| Chain Progress    | Checkpoints                         |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="New York, USA" %}
```
https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Singapore, Singapore" %}
```
https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network | JSON-RPC | WSS | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | -------- | --- | ------------------ | ------------- | -------------------- |
| Mainnet | ✅        | ❌   | ✅                  | ✅             | ✅                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir iota-quickstart && cd iota-api-quickstart && npm init --yes
```
{% endstep %}

{% step %}
### Install dependency

```bash
npm install axios
```
{% endstep %}

{% step %}
### Add code

{% code title="index.js" %}
```javascript
const axios = require('axios');
let data = JSON.stringify({
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "iota_getChainIdentifier",
  "params": []
});

let config = {
  method: 'post',
  maxBodyLength: Infinity,
  url: 'https://shared.us-east-1.getblock.io/<ACCESS_TOKEN>/',
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
{% endcode %}
{% endstep %}

{% step %}
### Run the script

```bash
node index.js
```
{% endstep %}

{% step %}
### Response

{% code overflow="wrap" %}
```bash
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "6364aad5"
}
```
{% endcode %}
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python(Request)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir iota-api-quickstart && cd iota-api-quickstart
python -m venv venv && source venv/bin/activate
```
{% endstep %}

{% step %}
### Install dependency

```bash
pip install requests
```
{% endstep %}

{% step %}
### Add code

{% code title="main.py" %}
```python
import requests
import json

url = "https://shared.us-east-1.getblock.io/<ACCESS_TOKEN>/"

payload = json.dumps({
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "iota_getChainIdentifier",
  "params": []
})
headers = {
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

```
{% endcode %}
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

## Available Methods

### Read API

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>iota_getChainIdentifier</td><td>Returns the chain identifier, a short hex string derived from the genesis checkpoint digest, that uniquely identifies the IOTA network (mainnet vs a testnet)</td></tr><tr><td>iota_getLatestCheckpointSequenceNumber</td><td>Returns the sequence number of the most recently executed checkpoint</td></tr><tr><td>iota_getCheckpoint</td><td>Returns a checkpoint by its sequence number or digest, including its timestamp, the transaction digests it contains, and its epoch</td></tr><tr><td>iota_getCheckpoints</td><td>Returns a paginated list of checkpoints in ascending or descending order from a cursor</td></tr><tr><td>iota_getObject</td><td>Returns the current state of an object by its ID, with selectable fields (type, owner, content, bcs, previous transaction)</td></tr><tr><td>iota_multiGetObjects</td><td>Returns the state of several objects in one call, each with the same display options</td></tr><tr><td>iota_tryGetPastObject</td><td>Returns an object as it existed at a specific version, if the node still retains that historical version</td></tr><tr><td>iota_tryMultiGetPastObjects</td><td>Returns several objects at specific past versions in one call</td></tr><tr><td>iota_getTransactionBlock</td><td>Returns a transaction block by digest, with selectable fields: input, effects, events, object changes, and balance changes</td></tr><tr><td>iota_multiGetTransactionBlocks</td><td>Returns several transaction blocks by digest in one call, with shared display options</td></tr><tr><td>iota_getEvents</td><td>Returns all events emitted by a given transaction block</td></tr><tr><td>iota_getTotalTransactionBlocks</td><td>Returns the total number of transaction blocks the network has executed since genesis</td></tr><tr><td>iota_getProtocolConfig</td><td>Returns the protocol configuration (feature flags and limits) for a given protocol version, defaulting to the latest</td></tr><tr><td>iota_isTransactionIndexedOnNode</td><td>Returns whether a given transaction digest has been indexed on the serving node, so a client knows when read-after-write queries will succeed</td></tr></tbody></table>

### Write API

| Method                           | Description                                                                                                                                                    |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| iota\_executeTransactionBlock    | Submits a signed transaction block (BCS-serialized) with its signatures for execution, and returns the requested effects                                       |
| iota\_dryRunTransactionBlock     | Dry-runs a transaction block against the current state and returns its predicted effects and gas cost, without committing it                                   |
| iota\_devInspectTransactionBlock | Simulates a transaction from its components for inspection, returning execution results including Move return values — without needing valid gas or signatures |

### Move Utils

| Method                                  | Description                                                                                                                            |
| --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| iota\_getNormalizedMoveModulesByPackage | Returns structured (normalized) representations of every Move module in a published package, including its structs and functions       |
| iota\_getNormalizedMoveModule           | Returns a structured representation of a single Move module: its structs, exposed functions, and friends                               |
| iota\_getNormalizedMoveFunction         | Returns a structured representation of a single Move function, including its parameter and return types and visibility                 |
| iota\_getNormalizedMoveStruct           | Returns a structured representation of a Move struct, including its fields and abilities                                               |
| iota\_getMoveFunctionArgTypes           | Returns the argument types (object vs pure) of a Move function, which a client needs to correctly place arguments when building a call |

### Coin Query

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>iotax_getBalance</td><td>Returns an address's total balance for a single coin type (IOTA by default), summed across all its coin objects</td></tr><tr><td>iotax_getAllBalances</td><td>Returns the total balance for every coin type an address owns, in one call</td></tr><tr><td>iotax_getCoins</td><td>Returns an address's individual Coin objects for a given coin type, paginated — the inputs you select when building a payment</td></tr><tr><td>iotax_getAllCoins</td><td>Returns all Coin objects an address owns across every coin type, paginated</td></tr><tr><td>iotax_getCoinMetadata</td><td>Returns the metadata (symbol, name, decimals, icon) for a coin type, used to display token amounts correctly</td></tr><tr><td>iotax_getTotalSupply</td><td>Returns the total supply of a coin type — for a regulated/custom coin, the amount its TreasuryCap has minted</td></tr><tr><td>iotax_getCirculatingSupply</td><td>Returns a summary of the circulating supply of the native IOTA token</td></tr></tbody></table>

### Extended API

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>iotax_getOwnedObjects</td><td>Returns the objects owned by an address, with an optional query filter (by type, struct, or owner) and display options, paginated</td></tr><tr><td>iotax_queryTransactionBlocks</td><td>Queries transaction blocks matching a filter (by sender, recipient, input/changed object, Move function, or time range), paginated</td></tr><tr><td>iotax_queryEvents</td><td>Queries events matching a filter (by package, module, event type, sender, or time range), paginated</td></tr><tr><td>iotax_getDynamicFields</td><td>Lists the dynamic fields attached to a parent object (for example, entries of an on-chain table), paginated</td></tr><tr><td>iotax_getDynamicFieldObject</td><td>Returns the full object stored at a specific dynamic field of a parent object, addressed by the field name</td></tr><tr><td>iotax_iotaNamesLookup</td><td>Resolves an IOTA name (the IOTA naming service) to its registry record, including the address it points to</td></tr><tr><td>iotax_iotaNamesReverseLookup</td><td>Resolves an address to its primary registered IOTA name, if any</td></tr><tr><td>iotax_getCurrentEpoch <em>(dedicated)</em></td><td>Returns information about the current epoch: its number, validators, start time, and reference gas price</td></tr><tr><td>iotax_getNetworkMetrics <em>(dedicated)</em></td><td>Returns live network metrics — current TPS, peak TPS, total objects, addresses, packages, and current epoch/checkpoint</td></tr><tr><td>iotax_getTotalTransactions <em>(dedicated)</em></td><td>Returns the total number of transactions recorded by the indexer</td></tr></tbody></table>

### Governance

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>iotax_getReferenceGasPrice</td><td>Returns the network's current reference gas price in NANOs, the baseline a transaction's gas price should meet or exceed to be included</td></tr><tr><td>iotax_getLatestIotaSystemStateV2</td><td>Returns the latest on-chain system state on protocol version 5 and above: epoch, protocol version, the active validator set, staking pools, and system parameters</td></tr><tr><td>iotax_getCommitteeInfo</td><td>Returns the validator committee (public keys and voting power) for an epoch, defaulting to the latest</td></tr><tr><td>iotax_getStakes</td><td>Returns all delegated stakes owned by an address, grouped by validator, including the principal and estimated rewards</td></tr><tr><td>iotax_getStakesByIds</td><td>Returns delegated stakes for the given StakedIota object IDs</td></tr><tr><td>iotax_getValidatorsApy</td><td>Returns the annualized percentage yield (APY) for each validator, for comparing staking returns</td></tr><tr><td>iotax_getTimelockedStakes</td><td>Returns all timelocked delegated stakes owned by an address — stakes whose principal is subject to a time lock</td></tr></tbody></table>

### Transaction Builder

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>unsafe_moveCall</td><td>Builds (but does not sign or submit) an unsigned transaction that calls a Move entry function, returning BCS tx bytes ready to sign</td></tr><tr><td>unsafe_pay</td><td>Builds an unsigned transaction that pays specified amounts of a coin to one or more recipients, selecting and splitting the given input coins</td></tr><tr><td>unsafe_payIota</td><td>Builds an unsigned transaction that pays IOTA to recipients, using the input IOTA coins themselves to pay gas (no separate gas coin)</td></tr><tr><td>unsafe_transferObject</td><td>Builds an unsigned transaction that transfers ownership of an object to a recipient address</td></tr><tr><td>unsafe_splitCoin</td><td>Builds an unsigned transaction that splits a coin object into several new coins of the specified amounts</td></tr><tr><td>unsafe_mergeCoins</td><td>Builds an unsigned transaction that merges one or more coins into a primary coin, consolidating balance into a single object</td></tr><tr><td>unsafe_requestAddStake</td><td>Builds an unsigned transaction that stakes IOTA coins with a validator, creating a delegated stake</td></tr><tr><td>unsafe_requestWithdrawStake</td><td>Builds an unsigned transaction that withdraws a previously delegated stake, returning principal and rewards</td></tr><tr><td>unsafe_publish</td><td>Builds an unsigned transaction that publishes a compiled Move package (its modules and dependencies) to the network</td></tr><tr><td>unsafe_batchTransaction</td><td>Builds an unsigned transaction that batches several operations (transfers, Move calls) into one atomic transaction</td></tr></tbody></table>

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [IOTA JSON-RPC API Reference](https://docs.iota.org/iota-api-ref)
* [IOTA Developer Documentation](https://docs.iota.org/)
* [IOTA Explorer](https://explorer.iota.org/)
* [IOTA Website](https://www.iota.org/)
