---
description: >-
  GetBlock provides fast and reliable access to Axelar nodes via JSON-RPC and
  REST APIs. Connect to the Axelar network without running your own
  infrastructure.
---

# Axelar (AXL)

Axelar is a decentralized cross-chain communication network built on the Cosmos SDK and secured by CometBFT (Tendermint) consensus with a delegated Proof-of-Stake validator set. Rather than serving a single application, Axelar connects blockchains: its validators observe events on external chains (both EVM and Cosmos), reach consensus, and produce threshold signatures that authorize transfers and general message passing (GMP) between chains. Developers use it to build interchain applications, move assets across ecosystems, and deploy Interchain Tokens. The native token is AXL, used for staking, fees, and governance. Because Axelar is a Cosmos SDK chain, GetBlock exposes it over two interfaces: JSON-RPC (the CometBFT node RPC) and REST (the Cosmos SDK LCD, including Axelar's cross-chain modules).

## Key Features

* **Cross-Chain Interoperability**: Connects EVM and Cosmos ecosystems with asset transfers and General Message Passing
* **Threshold Signatures**: Validators produce multi-party signatures that authorize actions on external chains
* **Cosmos SDK + CometBFT**: Deterministic single-block finality, DPoS validators, and IBC connectivity
* **Axelar Modules**: Chain-specific `nexus`, `evm`, `multisig`, and `reward` modules exposed over REST
* **Two Interfaces**: CometBFT JSON-RPC for consensus/block/tx data and broadcast; Cosmos REST for module queries
* **bech32 Addresses**: Accounts use the `axelar1…` address format; AXL is denominated in uaxl (10^-6 AXL)

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE API SPECIFICATION._

_GetBlock's API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical specifications are the CometBFT RPC (for JSON-RPC), the Cosmos SDK REST/gRPC (for standard modules), and the Axelar protobuf definitions (for the nexus, evm, multisig, and reward modules), published at_ [_docs.axelar.dev_](https://docs.axelar.dev/)_._
{% endhint %}

## Network Information

| Property        | Value                                               |
| --------------- | --------------------------------------------------- |
| Network Name    | Axelar (axelar-dojo-1)                              |
| Native Currency | AXL (1 AXL = 1,000,000 uaxl)                        |
| Consensus       | CometBFT (Tendermint) BFT, delegated Proof-of-Stake |
| Framework       | Cosmos SDK                                          |
| Address Format  | bech32 (axelar1…)                                   |
| Block Time      | \~6 seconds                                         |
| Finality        | Deterministic (single block)                        |

## Interfaces

| Interface | Transport                       | Use it for                                                                                         |
| --------- | ------------------------------- | -------------------------------------------------------------------------------------------------- |
| JSON-RPC  | CometBFT JSON-RPC 2.0 over HTTP | Consensus, block, and transaction data; transaction broadcast; `abci_query` for any module state   |
| REST      | Cosmos SDK REST (LCD) over HTTP | Module queries — bank, staking, gov, distribution, and Axelar's `nexus` / `evm` / `reward` modules |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network                 | JSON-RPC | REST | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ----------------------- | -------- | ---- | ------------------ | ------------- | -------------------- |
| Mainnet (axelar-dojo-1) | ✅        | ✅    | ✅                  | ❌             | ❌                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir axelar-api-quickstart && cd axelar-api-quickstart && npm init --yes
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
  "method": "status",
  "params": []
});

let config = {
  method: 'post',
  maxBodyLength: Infinity,
  url: 'https://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>',
  headers: { 
    'Content-Type': 'application/json', 
    'Accept': 'application/json'
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
    "jsonrpc": "2.0",
    "result": {
        "node_info": {
            "protocol_version": {
                "p2p": "8",
                "block": "11",
                "app": "0"
            },
            "id": "821fa0f7ce74a211c5f5ec93cc6cc301564b92b6",
            "listen_addr": "0.0.0.0:26656",
            "network": "axelar-dojo-1",
            "version": "0.38.25",
            "channels": "40202122233038606100",
            "moniker": "Tendermint",
            "other": {
                "tx_index": "on",
                "rpc_address": "tcp://0.0.0.0:26657"
            }
        },
        "sync_info": {
            "latest_block_hash": "5280FDEC95142F25A06A0654066AA08BE84C2036505D8C1667A2DDDEDC08535D",
            "latest_app_hash": "CA7F71F89A940520E975C463D08CE322F22E4DF3EAA592BF0B9E48FD79AF5164",
            "latest_block_height": "35202311",
            "latest_block_time": "2026-09-24T12:14:27.931417491Z",
            "earliest_block_hash": "86F60563C7D4C9172C67C85033FA62B58D3AED454465CAAF9E2821504D9FFA2B",
            "earliest_app_hash": "CD85F7AB6DEF0FE4D55D7CE3CFCE411D5EF2BBE1C36BD3339A95FAEE0EC1045A",
            "earliest_block_height": "34702311",
            "earliest_block_time": "2026-09-15T01:53:05.005785113Z",
            "catching_up": false
        },
        "validator_info": {
            "address": "0000000000000000000000000000000000000000",
            "pub_key": {
                "type": "tendermint/PubKeySecp256k1",
                "value": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA"
            },
            "voting_power": "0"
        }
    },
    "id": "getblock.io"
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
mkdir axelar-api-quickstart && cd axelar-api-quickstart
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

url = "https://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>"

payload = json.dumps({
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "status",
  "params": []
})
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json'
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

## APIs

* [JSON-RPC API](json-rpc-api/) — CometBFT consensus, block, tx, and `abci_query`
* [REST API](rest-api/) — Cosmos SDK and Axelar cross-chain module queries

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Axelar Documentation](https://docs.axelar.dev/)
* [CometBFT RPC](https://docs.cometbft.com/main/rpc/)
* [Cosmos SDK REST](https://docs.cosmos.network/)
* [Axelar Block Explorer (Axelarscan)](https://axelarscan.io/)
