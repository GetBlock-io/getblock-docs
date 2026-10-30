---
description: >-
  GetBlock provides fast and reliable access to Stable nodes via JSON-RPC and
  REST APIs. Connect to the Stable network without running your own
  infrastructure.
---

# Stable

Stable is a Layer-1 blockchain purpose-built for stablecoin payments: USDT0 is the native gas token, so fees are paid and denominated in a dollar-pegged asset. It pairs an EVM execution layer with StableBFT, a proof-of-stake consensus in the CometBFT (Tendermint) lineage. It exposes both the Ethereum `eth_*` JSON-RPC surface and the native Cosmos SDK / Tendermint interfaces. This reference documents all of them.

### Key Features

* **USDT0 Gas**: The native gas token is USDT0, a dollar-pegged asset — fees are stable in dollar terms
* **EVM + CometBFT**: An EVM execution layer on StableBFT (CometBFT/Tendermint-lineage) consensus — the `eth_*` surface runs alongside native Cosmos interfaces
* **Five API Interfaces**: EVM JSON-RPC (HTTP + WebSocket), Cosmos REST, Cosmos gRPC, and Tendermint RPC (HTTP + WebSocket)
* **Deterministic Finality**: CometBFT gives single-block finality — a result read at `latest` is already final
* **Dual Decimals**: USDT0 is 18 decimals as the native gas token and 6 decimals as the USDT0 ERC-20 token
* **Payments Focus**: Optimized for high-volume stablecoin transfers and payments

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE API SPECIFICATION._

_GetBlock's API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. Stable's EVM layer implements the standard Ethereum JSON-RPC interface (canonical spec at_ [_ethereum.org_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_); its Cosmos and Tendermint interfaces follow the Cosmos SDK and CometBFT specifications (_[_docs.cosmos.network_](https://docs.cosmos.network/)_,_ [_docs.cometbft.com_](https://docs.cometbft.com/main/spec/rpc/)_). Stable-specific details are documented at_ [_docs.stable.xyz_](https://docs.stable.xyz/)_._
{% endhint %}

## Network Information

| Property        | Value                                           |
| --------------- | ----------------------------------------------- |
| Network Name    | Stable Mainnet                                  |
| EVM Chain ID    | 988 (0x3dc)                                     |
| Native Currency | USDT0 (18-decimal native gas; 6-decimal ERC-20) |
| Consensus       | StableBFT (CometBFT / Tendermint lineage)       |
| Finality        | Deterministic (single block)                    |
| Fee Model       | EIP-1559 + legacy                               |

## Base URL

{% tabs %}
{% tab title="Singapore, Singapore" %}
```
https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Stable is served from the **Singapore** region only — use the Singapore base URL above. WebSocket is available at `wss://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/`.
{% endhint %}

## Available Interfaces

Stable pairs an EVM execution layer with a CometBFT consensus engine, so a node exposes several API interfaces. GetBlock serves all of the following; each is provisioned as its own endpoint, so choose the API interface when you create the endpoint in the GetBlock dashboard.

| Interface             | Protocol                             | Use it for                                                                                   |
| --------------------- | ------------------------------------ | -------------------------------------------------------------------------------------------- |
| EVM JSON-RPC (HTTP)   | Ethereum JSON-RPC 2.0 over HTTP      | Contract calls, transactions, and reads — the `eth_*` surface                                |
| EVM JSON-RPC (WS)     | Ethereum JSON-RPC 2.0 over WebSocket | `eth_subscribe` streams (newHeads, logs) and live EVM data                                   |
| Cosmos (REST)         | RESTful gRPC-gateway (LCD) over HTTP | Cosmos SDK module queries — bank, staking, governance — by bech32 address                    |
| Cosmos (gRPC)         | Protobuf gRPC services               | Typed Cosmos SDK queries for production backends                                             |
| Tendermint RPC (HTTP) | CometBFT JSON-RPC over HTTP          | Consensus, block, and transaction data; transaction broadcast; `abci_query` for module state |
| Tendermint RPC (WS)   | CometBFT JSON-RPC over WebSocket     | Event subscriptions (new blocks, transactions) over WebSocket                                |

The Cosmos REST and gRPC interfaces carry the same Cosmos SDK query set over different transports (REST is an HTTP/JSON gateway in front of gRPC). Tendermint RPC exposes the CometBFT consensus engine directly and is where transactions are broadcast and low-level block and `abci_query` data is read.

## Interface Endpoints

{% tabs %}
{% tab title="EVM JSON-RPC" %}
```bash
https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Cosmos REST" %}
```bash
https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Tendermint RPC" %}
```bash
https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Cosmos gRPC" %}
```
shared.ap-southeast-1.getblock.io:443   (TLS, access token passed as request metadata)
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard. Each interface is provisioned as its own endpoint — select the API interface when you create the endpoint. EVM JSON-RPC, Cosmos REST, and Tendermint RPC are HTTP; gRPC connects to a `host:port` target over TLS.
{% endhint %}

## Supported Networks

| Network | EVM Chain ID | EVM JSON-RPC (HTTP) | EVM JSON-RPC (WS) | Cosmos (REST) | Cosmos (gRPC) | Tendermint RPC (HTTP) | Tendermint RPC (WSS) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | ------------ | ------------------- | ----------------- | ------------- | ------------- | --------------------- | -------------------- | ------------------ | ------------- | -------------------- |
| Mainnet | 988          | ✅                   | ✅                 | ✅             | ✅             | ✅                     | ✅                    | ❌                  | ❌             | ✅                    |

## APIs

* [EVM JSON-RPC API](evm-json-rpc/) — the Ethereum-compatible `eth_*` surface (HTTP + WebSocket)
* [Cosmos REST API](cosmos-rest/) — Cosmos SDK module queries over HTTP/JSON
* [Cosmos gRPC API](cosmos-grpc.md) — the same query set over Protocol Buffers
* [Tendermint RPC API](tendermint-rpc/) — CometBFT consensus, block, tx, and `abci_query` (HTTP + WebSocket)

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir stable-api-quickstart && cd stable-api-quickstart && npm init --yes
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

const url = 'https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/';

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
Latest block: 44968084
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
mkdir stable-api-quickstart && cd stable-api-quickstart
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

url = "https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/"

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

* [Stable Documentation](https://docs.stable.xyz/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [CometBFT RPC Reference](https://docs.cometbft.com/main/spec/rpc/)
* [Cosmos SDK Documentation](https://docs.cosmos.network/)
* [Stablescan Explorer](https://stablescan.xyz/)
