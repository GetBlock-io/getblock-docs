---
description: >-
  GetBlock provides fast and reliable access to TAC nodes via EVM JSON-RPC,
  Cosmos REST, Cosmos gRPC, and Tendermint RPC APIs. Connect to the TAC network
  without running your own infrastructure.
---

# TAC

TAC (TON Application Chain) is an EVM-compatible Layer-1 that connects the TON ecosystem to EVM applications: it lets TON users interact with Solidity dApps while developers build with standard Ethereum tooling. TAC is built on the Cosmos SDK with a cosmos/evm execution layer and CometBFT (Tendermint) consensus, so it exposes both the Ethereum `eth_*` JSON-RPC surface and the full Cosmos SDK interfaces. The native gas token is TAC.

## Key Features

* **EVM + Cosmos L1**: A Cosmos SDK chain with a cosmos/evm execution layer — Solidity contracts and the `eth_*` surface run alongside native Cosmos modules
* **TON Connectivity**: Purpose-built to bridge the TON ecosystem to EVM applications
* **CometBFT Consensus**: Deterministic single-block finality — a result read at `latest` is already final
* **Six API Interfaces**: EVM JSON-RPC (HTTP + WS), Cosmos REST, Cosmos gRPC, and Tendermint RPC (HTTP + WS)
* **TAC Gas Token**: Native gas is paid in TAC (18 decimals); EIP-1559 fee-market pricing applies on the EVM layer
* **Ethereum Tooling**: Compatible with MetaMask, Foundry, Hardhat, Remix, Ethers.js, and Viem

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE API SPECIFICATION._

_GetBlock's API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. TAC's EVM layer implements the standard Ethereum JSON-RPC interface (canonical spec at_ [_ethereum.org_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_); its Cosmos and Tendermint interfaces follow the Cosmos SDK and CometBFT specifications (_[_docs.cosmos.network_](https://docs.cosmos.network/)_,_ [_docs.cometbft.com_](https://docs.cometbft.com/main/spec/rpc/)_). TAC-specific details are documented at_ [_docs.tac.build_](https://docs.tac.build/)_._
{% endhint %}

## Network Information

| Property        | Value                        |
| --------------- | ---------------------------- |
| Network Name    | TAC Mainnet                  |
| EVM Chain ID    | 239 (0xef)                   |
| Cosmos Chain ID | tacchain\_2390-1             |
| Native Currency | TAC (18 decimals)            |
| Framework       | Cosmos SDK + cosmos/evm      |
| Consensus       | CometBFT (Tendermint) BFT    |
| Finality        | Deterministic (single block) |
| Fee Model       | EIP-1559 (feemarket)         |

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

## Available Interfaces

TAC is a Cosmos SDK chain with an embedded cosmos/evm layer, so a TAC node exposes several API interfaces. GetBlock serves all of the following; each is provisioned as its own endpoint, so choose the API interface when you create the endpoint in the GetBlock dashboard.

| Interface             | Protocol                             | Use it for                                                                                   |
| --------------------- | ------------------------------------ | -------------------------------------------------------------------------------------------- |
| EVM JSON-RPC (HTTP)   | Ethereum JSON-RPC 2.0 over HTTP      | Contract calls, transactions, and reads — the `eth_*` surface                                |
| EVM JSON-RPC (WS)     | Ethereum JSON-RPC 2.0 over WebSocket | `eth_subscribe` streams (newHeads, logs) and live EVM data                                   |
| Cosmos (REST)         | RESTful gRPC-gateway (LCD) over HTTP | Cosmos SDK module queries — bank, staking, governance — by bech32 (`tac1…`) address          |
| Cosmos (gRPC)         | Protobuf gRPC services               | Typed Cosmos SDK queries for production backends                                             |
| Tendermint RPC (HTTP) | CometBFT JSON-RPC over HTTP          | Consensus, block, and transaction data; transaction broadcast; `abci_query` for module state |
| Tendermint RPC (WS)   | CometBFT JSON-RPC over WebSocket     | Event subscriptions (new blocks, transactions) over WebSocket                                |

The Cosmos REST and gRPC interfaces carry the same Cosmos SDK query set over different transports (REST is an HTTP/JSON gateway in front of gRPC). Tendermint RPC exposes the CometBFT consensus engine directly and is where transactions are broadcast and low-level block and `abci_query` data is read. The Cosmos interfaces use the bech32 `tac1…` address encoding; a TAC account's `0x` and `tac1…` addresses are two encodings of the same key.

## Interface Endpoints

{% tabs %}
{% tab title="EVM JSON-RPC" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Cosmos REST" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
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
Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard. Each interface is provisioned as its own endpoint — select the API interface when you create the endpoint. EVM JSON-RPC, Cosmos REST, and Tendermint RPC are HTTP; gRPC connects to a `host:port` target over TLS.
{% endhint %}

## Supported Networks

| Network                | EVM Chain ID | EVM JSON-RPC (HTTP) | EVM JSON-RPC (WS) | Cosmos (REST) | Cosmos (gRPC) | Tendermint RPC (HTTP) | Tendermint RPC (WSS) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ---------------------- | ------------ | ------------------- | ----------------- | ------------- | ------------- | --------------------- | -------------------- | ------------------ | ------------- | -------------------- |
| Mainnet                | 239          | ✅                   | ✅                 | ✅             | ✅             | ✅                     | ✅                    | ✅                  | ✅             | ✅                    |
| Saint Petersburg (SPB) | 2391         | ✅                   | ✅                 | ✅             | ✅             | ✅                     | ✅                    | ✅                  | ❌             | ❌                    |

## APIs

* [EVM JSON-RPC API](evm-json-rpc/) — the Ethereum-compatible `eth_*` surface (HTTP + WebSocket)
* [Cosmos REST API](cosmos-rest/) — Cosmos SDK module queries over HTTP/JSON
* [Cosmos gRPC API](/broken/pages/d4391bebc442b05039bd843fefc7b570343ee186) — the same query set over Protocol Buffers
* [Tendermint RPC API](tendermint-rpc/) — CometBFT consensus, block, tx, and `abci_query` (HTTP + WebSocket)

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir tac-api-quickstart && cd tac-api-quickstart && npm init --yes
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

// EVM JSON-RPC: latest block number
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

{% step %}
### Response

{% code overflow="wrap" %}
```bash
Latest block: 27366469
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
mkdir tac-api-quickstart && cd tac-api-quickstart
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

# Tendermint RPC: node status (chain id, latest height, sync state)
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

* [TAC Documentation](https://docs.tac.build/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [CometBFT RPC Reference](https://docs.cometbft.com/main/spec/rpc/)
* [Cosmos SDK Documentation](https://docs.cosmos.network/sdk/latest/api-reference/grpc)
* [TAC Explorer](https://explorer.tac.build/)
