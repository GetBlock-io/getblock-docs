---
description: >-
  GetBlock provides fast and reliable access to TON RPC nodes via JSON-RPC API.
  Connect to the TON network without running your own infrastructure.
---

# The Open Network (TON)

The Open Network (TON) is a high-performance, decentralized Layer 1 blockchain originally designed by Telegram developers Nikolai and Pavel Durov and now maintained by the independent TON Foundation and community. Built around a sharded multi-blockchain architecture and Byzantine Fault Tolerant Proof-of-Stake consensus, TON is designed to scale horizontally through dynamic sharding while maintaining \~5-second block times on its masterchain. The network is natively integrated with Telegram’s 800M+ user base and powers payments, DeFi, NFTs, and decentralized services such as TON DNS, TON Sites, and TON Storage.

### Key Features

* **Dynamic Sharding**: Masterchain coordinates a configurable set of workchains, each of which can split into shardchains as load increases — enabling horizontal scalability
* **High Throughput**: Architecture designed for parallel transaction processing across shards, with documented benchmarks above 100,000 TPS
* **Fast Finality**: \~5 second masterchain block times with deterministic finality
* **BFT Proof-of-Stake**: Validators are elected from stakers and rotate every election cycle
* **TON Virtual Machine (TVM)**: Stack-based VM that runs smart contracts compiled from FunC, Tact, or Tolk — not EVM-compatible
* **Native Token Standards**: Jettons (TIP-3 fungible tokens) and NFTs (TIP-4 non-fungible tokens)
* **Toncoin (TON)**: Native gas and staking token, 9 decimals (1 TON = 1,000,000,000 nanotons)
* **Telegram Integration**: First-class integration with the Telegram Mini Apps platform and TON Connect wallet protocol
* **Low Fees**: Sub-cent transaction fees for typical wallet and contract operations
* **Decentralized Services**: TON DNS, TON Sites, TON Storage, and TON Proxy for fully on-chain web infrastructure

{% hint style="info" %}
**TECHNICAL DISCLAIMER: AUTHORITATIVE TON HTTP API SPECIFICATION**

_GetBlock’s TON API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for the TON HTTP API (v2) is maintained by TON Foundation and published at_ [_toncenter.com/api/v2/_](https://toncenter.com/api/v2/) _with the upstream source at_ [_github.com/toncenter/ton-http-api_](https://github.com/toncenter/ton-http-api)_. For underlying protocol details, consult the official TON documentation at_ [_docs.ton.org_](https://docs.ton.org/)_._
{% endhint %}

## Network Information

| Property          | Value                                                  |
| ----------------- | ------------------------------------------------------ |
| Network Name      | TON Mainnet                                            |
| Native Currency   | Toncoin (TON)                                          |
| Decimals          | 9 (1 TON = 1,000,000,000 nanotons)                     |
| Block Time        | \~5 seconds (masterchain)                              |
| Consensus         | Byzantine Fault Tolerant Proof-of-Stake                |
| Architecture      | Sharded — masterchain (workchain `-1`) + workchains    |
| Smart Contract VM | TVM (TON Virtual Machine)                              |
| EVM Compatible    | No                                                     |
| Address Format    | User-friendly (`EQ…` / `UQ…`) or raw (`workchain:hex`) |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

The TON HTTP API exposes every method on **two parallel transports** behind your endpoint:

* **REST (GET)**: `GET https://go.getblock.io/<ACCESS-TOKEN>/<method>?<query-params>`
* **JSON-RPC (POST)**: `POST https://go.getblock.io/<ACCESS-TOKEN>/jsonRPC` with a standard JSON-RPC 2.0 body

A handful of state-mutating methods (`sendBoc`, `sendBocReturnHash`, `runGetMethod`, `runGetMethodStd`, `estimateFee`) are POST-only.

## Supported Networks

| Network | JSON-RPC | REST (HTTP API (v4) ) | JSON-RPC (v3) | Frankfurt, Germany |
| ------- | -------- | --------------------- | ------------- | ------------------ |
| Mainnet | ✅        | ✅                     | ✅             | ✅                  |

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
mkdir ton-api-quickstart
cd ton-api-quickstart
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
const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/getMasterchainInfo',
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

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
{% endstep %}

{% step %}
#### Run the script

```bash
node index.js
```

Expected output (example):

```json
{
    "ok": true,
    "result": {
        "@type": "blocks.masterchainInfo",
        "last": {
            "@type": "ton.blockIdExt",
            "workchain": -1,
            "shard": "-9223372036854775808",
            "seqno": 66745702,
            "root_hash": "xO7AlBfHBD18HQxua03QQKb1BMZIOJjPtVVj7rRyxPk=",
            "file_hash": "nzRCfV2w6GKyA6DW61muIVTdLkbkSwozhYDWERj1hmw="
        },
        "state_root_hash": "p58IFlKiohPOi/aiqSW9M8o2AXcc6ZUwANhJlEE5cVw=",
        "init": {
            "@type": "ton.blockIdExt",
            "workchain": -1,
            "shard": "0",
            "seqno": 0,
            "root_hash": "F6OpKZKqvqeFp6CQmFomXNMfMj2EnaUSOXN+Mh+wVWk=",
            "file_hash": "XplPz01CXAps5qeSWUtxcyBfdAo5zVb1N979KLSKD24="
        },
        "@extra": "1778774352.215111:0:0.6613766254069332"
    }
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
mkdir ton-api-quickstart
cd ton-api-quickstart
```
{% endstep %}

{% step %}
#### Create and activate a virtual environment

```bash
python -m venv venv
source venv/bin/activate

# On Windows, use:
venv\Scripts\activate
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/getMasterchainInfo"
headers = {
    'Content-Type': 'application/json'
}
response = requests.post(url, headers=headers)
print(response.text)
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
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

GetBlock provides access to the full TON HTTP API v2 surface — every method is available both as a REST `GET` and as a JSON-RPC `POST` to `/jsonRPC`, except for the POST-only methods marked below.

### Account Methods

| Method                        | Description                                                |
| ----------------------------- | ---------------------------------------------------------- |
| getAddressInformation         | Get basic information about an address                     |
| getExtendedAddressInformation | Get extended information about an address                  |
| getWalletInformation          | Get wallet-specific information about an address           |
| getAddressBalance             | Get address balance in nanotons                            |
| getAddressState               | Get address state (`active`, `uninitialized`, or `frozen`) |
| getShardAccountCell           | Get the raw TVM cell representing the shard account        |
| getTokenData                  | Get Jetton or NFT metadata from a token smart contract     |

### Block Methods

| Method                        | Description                                      |
| ----------------------------- | ------------------------------------------------ |
| getMasterchainInfo            | Get up-to-date masterchain state                 |
| getMasterchainBlockSignatures | Get validator signatures for a masterchain block |
| getShardBlockProof            | Get the Merkle proof of a shardchain block       |
| getConsensusBlock             | Get the block confirmed by consensus             |
| lookupBlock                   | Look up a block by seqno, shard, and workchain   |
| getShards                     | Get shards info by masterchain block seqno       |
| getBlockHeader                | Get block header information                     |
| getOutMsgQueueSize            | Get the size of the outbound message queue       |

### Transaction Methods

| Method                  | Description                                                           |
| ----------------------- | --------------------------------------------------------------------- |
| getTransactions         | Get transactions for a specified address                              |
| getTransactionsStd      | Standardized version of `getTransactions`                             |
| getBlockTransactions    | Get transactions in a specified block                                 |
| getBlockTransactionsExt | Get transactions in a block with extended information                 |
| tryLocateTx             | Locate the outgoing transaction at the destination by inbound message |
| tryLocateResultTx       | Same as `tryLocateTx` — locate the outgoing transaction               |
| tryLocateSourceTx       | Locate the source transaction by destination transaction parameters   |

### Configuration Methods

| Method         | Description                                     |
| -------------- | ----------------------------------------------- |
| getConfigParam | Get a single blockchain configuration parameter |
| getConfigAll   | Get all blockchain configuration parameters     |
| getLibraries   | Get library code by their hashes                |

### Run-Method (POST only)

| Method          | Description                            |
| --------------- | -------------------------------------- |
| runGetMethod    | Run a get-method of a smart contract   |
| runGetMethodStd | Standardized version of `runGetMethod` |

### Send Methods (POST only)

| Method            | Description                                       |
| ----------------- | ------------------------------------------------- |
| sendBoc           | Send a Bag of Cells (signed message) to the chain |
| sendBocReturnHash | Send a BoC and return its hash for tracking       |
| estimateFee       | Estimate fees for a given message                 |

### Utility Methods

| Method        | Description                              |
| ------------- | ---------------------------------------- |
| detectAddress | Get all possible forms of a TON address  |
| detectHash    | Get all possible forms of a 256-bit hash |
| packAddress   | Pack an address into base64 form         |
| unpackAddress | Unpack an address from base64 form       |

### JSON-RPC Endpoint

| Method  | Description                                                                  |
| ------- | ---------------------------------------------------------------------------- |
| jsonRPC | Generic JSON-RPC 2.0 entrypoint — all of the methods above are callable here |

## Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Official TON Documentation](https://docs.ton.org/)
* [TON HTTP API v2 Reference](https://toncenter.com/api/v2/)
* [TON HTTP API GitHub](https://github.com/toncenter/ton-http-api)
* [Tonviewer Block Explorer](https://tonviewer.com/)
* [TONScan Block Explorer](https://tonscan.org/)
* [TON Foundation](https://ton.foundation/)
* [@ton/ton (Official JavaScript SDK)](https://github.com/ton-org/ton)
* [pytoniq (Python SDK)](https://github.com/yungwine/pytoniq)
* [TON Connect Protocol](https://docs.ton.org/develop/dapps/ton-connect/overview)
