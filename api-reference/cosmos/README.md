---
description: >-
  GetBlock provides fast and reliable access to Cosmos nodes through REST, gRPC,
  and Tendermint RPC APIs.
---

# Cosmos

Cosmos Hub is the flagship blockchain of the Cosmos ecosystem — the "Internet of Blockchains" — and the original implementation of the Cosmos SDK and CometBFT (formerly Tendermint) consensus.

It secures the ATOM token, hosts the Inter-Blockchain Communication protocol (IBC), and provides Interchain Security to consumer chains. Cosmos Hub is not an EVM chain; GetBlock serves it through its native Cosmos interfaces: Cosmos REST, Cosmos gRPC, and Tendermint (CometBFT) RPC over HTTP and WebSocket.

### Key Features

* **CometBFT Consensus**: Deterministic instant finality (one block = final) via Tendermint-style BFT — no fork choice, no probabilistic confirmation windows
* **Cosmos SDK Architecture**: Modular chain built around specialized modules (`bank`, `staking`, `gov`, `distribution`, `slashing`, `mint`, `ibc`)
* **Four API Interfaces**: Cosmos REST, Cosmos gRPC, and Tendermint RPC over HTTP and WebSocket
* **ATOM Token**: Native staking, governance, and IBC fee asset, denominated in `uatom` (1 ATOM = 1,000,000 uatom — 6 decimals)
* **Bech32 Addresses**: Human-readable `cosmos1…` accounts and `cosmosvaloper1…` validator addresses
* **IBC Connectivity**: First-class interoperability with 100+ Cosmos ecosystem chains

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE API SPECIFICATION._

_GetBlock's API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical specification for the CometBFT RPC interface is maintained by the CometBFT team at_ [_docs.cometbft.com_](https://docs.cometbft.com/main/spec/rpc/)_; Cosmos SDK module, REST, and gRPC details are published at_ [_docs.cosmos.network_](https://docs.cosmos.network/)_._
{% endhint %}

## Network Information

| Property        | Value                                                                                       |
| --------------- | ------------------------------------------------------------------------------------------- |
| Network Name    | Cosmos Hub Mainnet                                                                          |
| Chain ID        | cosmoshub-4                                                                                 |
| Native Currency | ATOM                                                                                        |
| Denomination    | uatom (1 ATOM = 1,000,000 uatom — 6 decimals)                                               |
| Consensus       | CometBFT (Tendermint) BFT                                                                   |
| Finality        | Deterministic (single block)                                                                |
| Address Format  | Bech32 `cosmos1…` (validator: `cosmosvaloper1…`)                                            |
| Block Explorer  | [mintscan.io/cosmos](https://www.mintscan.io/cosmos), [atomscan.com](https://atomscan.com/) |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Cosmos Hub is served from the **Frankfurt** region only — use the Frankfurt base URL above. Both the mainnet (`cosmoshub-4`) and the public testnet (`provider`) are available there; each is provisioned as its own endpoint in the GetBlock dashboard.
{% endhint %}

## Available Interfaces

Cosmos Hub is a Cosmos SDK chain, so a node exposes several API interfaces (there is no Ethereum JSON-RPC — Cosmos Hub is not EVM-compatible). GetBlock serves all of the following; each is provisioned as its own endpoint, so choose the API interface when you create the endpoint in the GetBlock dashboard.

| Interface             | Protocol                             | Use it for                                                                                                 |
| --------------------- | ------------------------------------ | ---------------------------------------------------------------------------------------------------------- |
| Cosmos (REST)         | RESTful gRPC-gateway (LCD) over HTTP | Cosmos SDK module queries — bank, staking, governance, distribution, mint — by bech32 (`cosmos1…`) address |
| Cosmos (gRPC)         | Protobuf gRPC services               | Typed Cosmos SDK queries for production backends                                                           |
| Tendermint RPC (HTTP) | CometBFT JSON-RPC over HTTP          | Consensus, block, and transaction data; transaction broadcast; `abci_query` for module state               |
| Tendermint RPC (WS)   | CometBFT JSON-RPC over WebSocket     | Event subscriptions (new blocks, transactions) over WebSocket                                              |

The Cosmos REST and gRPC interfaces carry the same Cosmos SDK query set over different transports (REST is an HTTP/JSON gateway in front of gRPC). Tendermint RPC exposes the CometBFT consensus engine directly and is where transactions are broadcast and low-level block and `abci_query` data is read.

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
Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard. Cosmos REST and Tendermint RPC are HTTP (WebSocket subscriptions use `wss://…/websocket`); gRPC connects to a `host:port` target over TLS.
{% endhint %}

## Supported Networks

| Network | Chain ID    | Cosmos (REST) | Cosmos (gRPC) | Tendermint RPC (HTTP) | Tendermint RPC (WSS) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | ----------- | ------------- | ------------- | --------------------- | -------------------- | ------------------ | ------------- | -------------------- |
| Mainnet | cosmoshub-4 | ✅             | ✅             | ✅                     | ✅                    | ✅                  | ❌             | ❌                    |
| Testnet | provider    | ✅             | ✅             | ✅                     | ✅                    | ✅                  | ❌             | ❌                    |

## APIs

* [Cosmos REST API](cosmos-rest-api/) — Cosmos SDK module queries over HTTP/JSON
* [Cosmos gRPC API](cosmos-grpc.md) — the same query set over Protocol Buffers
* [Tendermint RPC API](tendermint-rpc-api/) — CometBFT consensus, block, tx, and `abci_query` (HTTP + WebSocket)

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir cosmos-api-quickstart && cd cosmos-api-quickstart && npm init --yes
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

// Tendermint RPC: node status (chain id, latest height, sync state)
const payload = {
  jsonrpc: '2.0',
  method: 'status',
  params: [],
  id: 'getblock.io'
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(JSON.stringify(response.data.result.sync_info, null, 2)))
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
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
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
            "version": "0.38.25",
            "channels": "40202122233038606100",
            "moniker": "9f5085e21705",
            "other": {
                "tx_index": "on",
                "rpc_address": "tcp://0.0.0.0:26657"
            }
        },
        "sync_info": {
            "latest_block_hash": "FA290CFAFD4A6D243E9F3C36631EE32F2EA59DF033451A5290AF887863B00072",
            "latest_app_hash": "F47458A8D73EDB5A80531754ECDDC4FB6C0ED50995FCD101BC10938E0D98CCF0",
            "latest_block_height": "33618757",
            "latest_block_time": "2026-10-28T14:46:06.560439729Z",
            "earliest_block_hash": "",
            "earliest_app_hash": "",
            "earliest_block_height": "0",
            "earliest_block_time": "1970-01-01T00:00:00Z",
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
{% endcode %}
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python(Request)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir cosmos-api-quickstart && cd cosmos-api-quickstart
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

# Cosmos REST: an account's ATOM balance
address = "cosmos1xv9tklw7d82sezh9haa573wufgy59vmwe6xxe5"
response = requests.get(f"{url}cosmos/bank/v1beta1/balances/{address}")
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

* [Official Cosmos Documentation](https://docs.cosmos.network/)
* [CometBFT RPC Reference](https://docs.cometbft.com/main/spec/rpc/)
* [Cosmos REST API Reference](https://docs.cosmos.network/api)
* [Mintscan Explorer](https://www.mintscan.io/cosmos)
* [Cosmos Chain Registry](https://github.com/cosmos/chain-registry)
