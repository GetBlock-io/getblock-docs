---
description: >-
  GetBlock provides fast and reliable access to Cronos nodes via JSON-RPC API.
  Connect to the Cronos network without running your own infrastructure.
---

# Cronos (CRO)

Cronos is an EVM-compatible Layer 1 blockchain built by Crypto.com on the Cosmos SDK with Ethermint, secured by CometBFT (Tendermint) BFT consensus for fast, deterministic finality. It runs a standard Ethereum execution layer, so Solidity contracts, the `eth_*` JSON-RPC surface, and tooling such as Foundry, Hardhat, Ethers.js, and Viem work unchanged, while IBC connectivity links it to the wider Cosmos ecosystem. The native gas token is CRO, and Cronos supports EIP-1559 fee-market pricing through Ethermint's feemarket module. This reference document describes the Ethereum-compatible `eth_*` surface of Cronos EVM (chain ID 25), which is distinct from Cronos zkEVM (chain ID 388).

### Key Features

* **EVM Compatibility**: Standard Ethereum contracts, JSON-RPC, and tooling run unchanged on the Ethermint execution layer
* **Cosmos SDK Foundation**: Built on the Cosmos SDK with IBC connectivity to the broader Cosmos ecosystem
* **CometBFT Consensus**: Tendermint BFT gives fast, deterministic single-block finality with no probabilistic reorgs
* **EIP-1559 Fees**: Ethermint's feemarket module provides base-fee pricing; `eth_feeHistory` and priority fees apply
* **CRO Gas Token**: Native gas is paid in CRO (18 decimals), with low, sub-cent transaction costs
* **Ethereum Tooling**: Compatible with MetaMask, Foundry, Hardhat, Remix, Ethers.js, and Viem

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. Cronos implements the standard Ethereum JSON-RPC interface via Ethermint; the canonical specification for these methods is the Ethereum JSON-RPC specification at_ [_ethereum.org_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_, and Cronos-specific behaviour is documented at_ [_docs.cronos.org_](https://docs.cronos.org/)_._
{% endhint %}

## Network Information

| Property        | Value                        |
| --------------- | ---------------------------- |
| Network Name    | Cronos EVM                   |
| Chain ID        | 25                           |
| Native Currency | CRO                          |
| EVM Compatible  | Yes (Ethermint)              |
| Framework       | Cosmos SDK + Ethermint       |
| Consensus       | CometBFT (Tendermint) BFT    |
| Block Time      | \~5-6 seconds                |
| Finality        | Deterministic (single block) |
| Fee Model       | EIP-1559 (feemarket)         |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

## Available Interfaces

Cronos is a Cosmos SDK chain with an embedded Ethermint EVM, so a Cronos node exposes several API interfaces beyond the Ethereum one. GetBlock serves all of the following for Cronos; each is provisioned as its own endpoint, so choose the API interface when you create the endpoint in the GetBlock dashboard.

| Interface             | Protocol                             | Use it for                                                                                   |
| --------------------- | ------------------------------------ | -------------------------------------------------------------------------------------------- |
| EVM JSON-RPC (HTTP)   | Ethereum JSON-RPC 2.0 over HTTP      | Contract calls, transactions, and reads — the surface this reference documents               |
| EVM JSON-RPC (WS)     | Ethereum JSON-RPC 2.0 over WebSocket | `eth_subscribe` streams (newHeads, logs) and live EVM data                                   |
| Cosmos (REST)         | RESTful gRPC-gateway (LCD) over HTTP | Cosmos SDK module queries — bank, staking, governance, IBC — by bech32 (`crc1…`) address     |
| Cosmos (gRPC)         | Protobuf gRPC services               | Typed Cosmos SDK queries for production backends                                             |
| Tendermint RPC (HTTP) | CometBFT JSON-RPC over HTTP          | Consensus, block, and transaction data; transaction broadcast; `abci_query` for module state |
| Tendermint RPC (WS)   | CometBFT JSON-RPC over WebSocket     | Event subscriptions (new blocks, transactions) over WebSocket                                |

The Cosmos REST and gRPC interfaces carry the same Cosmos SDK query set over different transports (REST is an HTTP/JSON gateway in front of gRPC). Tendermint RPC exposes the CometBFT consensus engine directly and is where transactions are broadcast and low-level block and `abci_query` data is read. All three use the bech32 `crc1…` address encoding; a Cronos account's `0x` and `crc1…` addresses are two encodings of the same key.

## Interface Endpoints

{% tabs %}
{% tab title="Cosmos REST" %}
{% code overflow="wrap" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endcode %}
{% endtab %}

{% tab title="Tendermint RPC" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Cosmos gRPC" %}
```bash
shared.eu-central-1.getblock.io:443   (TLS, access token passed as request metadata)
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard. Each interface is provisioned as its own endpoint — select the API interface when you create the endpoint. Cosmos REST and Tendermint RPC are HTTP; gRPC connects to a `host:port` target over TLS. Confirm the exact gRPC target and access token mechanism (metadata header vs. path) in the dashboard.
{% endhint %}

## Supported Networks

| Network | Chain ID | EVM JSON-RPC (HTTP) | EVM JSON-RPC (WS) | Cosmos (REST) | Cosmos (gRPC) | Tendermint RPC (HTTP) | Tendermint RPC (WSS) | Frankfurt, Germany |
| ------- | -------- | ------------------- | ----------------- | ------------- | ------------- | --------------------- | -------------------- | ------------------ |
| Mainnet | 25       | ✅                   | ✅                 | ✅             | ✅             | ✅                     | ✅                    | ✅                  |

## APIs

* [Cosmos REST API](cosmos-rest-api-cronos/) — Cosmos SDK module queries over HTTP/JSON
* [Cosmos gRPC](cosmos-grpc-api-cronos/) — the same query set over Protocol Buffers
* [Tendermint RPC API](tendermint-rpc-api-cronos/) — CometBFT consensus, block, tx, and `abci_query`
* [EVM JSON-RPC API](evm-json-rpc-cronos/) — the Ethereum-compatible `eth_*` surface (documented separately)

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir cronos-api-quickstart && cd cronos-api-quickstart && npm init --yes
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

const url = 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  method: 'eth_blockNumber',
  params: [],
  id: 'getblock.io'
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => {
  console.log('Latest block:', parseInt(response.data.result, 16));
})
.catch(error => console.error(error));
```
{% endcode %}
{% endstep %}

{% step %}
### Run the script

```bash
node index.js
```
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python(Request)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir cronos-api-quickstart && cd cronos-api-quickstart
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

url = "https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/"

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

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Cronos Documentation](https://docs.cronos.org/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [Cronos EVM Explorer](https://explorer.cronos.org/)
* [Cronos Bridge](https://cronos.org/bridge)
* [Cronos Website](https://cronos.org/)
