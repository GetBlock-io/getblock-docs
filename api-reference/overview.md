---
description: >-
  GetBlock API docs: RPC endpoints, SDK guides, and developer tools. Everything
  you need to connect and build on 70+ blockchain networks
icon: hammer
---

# Overview

## Getting Started with GetBlock

To get started, follow these steps:

1. Sign up: Create an account to access your Access Token.
2. Generate access token: Navigate to the dashboard, and generate your first access token for API authentication.
3. Choose the blockchain name and type: Select the blockchain network you want to interact with, set up testnet/mainnet, and choose the interface you’re going to use.
4. Send your first request:

```python
import requests
url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = { "Content-Type": "application/json" }
payload = { "jsonrpc": "2.0", "method": "eth_blockNumber", "params": [], "id": "getblock.io" }
response = requests.post(url, headers=headers, json=payload)
print(response.json())
```

### Key Features of GetBlock

GetBlock is offering one of the most comprehensive APIs toolkits in the segment supporting hundreds of dApps with fast and reliable connection to blockchain nodes.

#### 70+ blockchains in store

* Seamless connection to full and archive nodes
* Shared and dedicated nodes: Tailored for your dApp’s needs.
* Programmable and non-programmable blockchains
* L1 and L2 protocols
* EVM and non-EVM networks

See full list: [https://getblock.io/nodes/](https://getblock.io/nodes/)\
\
All mainstream RPC interfaces

* WebSockets
* JSON RPC
* GraphQL
* REST API
* And more&#x20;

Add them in Lego-like manner: [https://account.getblock.io/](https://account.getblock.io/)&#x20;

#### Industry-leading suite of add-ons and ready-made APIs

* DAS API
* Firehose
* Blockbook
* Yellowstone Geyser

Need more? Don’t hesitate to contact sales: [https://getblock.io/contact/](https://getblock.io/contact/)&#x20;

## Examples for Console REST API Requests

#### cURL

Most \*nix-based systems come with cURL pre-installed. cURL is a command-line tool and library for transferring data with URLs. To check if cURL is installed, run the following command:

```json
curl -h
```

Example of requesting the latest block number using the GetBlock API and cURL:

```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
    "id": "getblock.io"
}'
```

#### Python

To run Python examples, ensure Python is installed along with the requests package. You can install the package using the following command:

```json
python -m pip install requests
```

Example:

```python
import requests
url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = { "Content-Type": "application/json" }
payload = { "jsonrpc": "2.0", "method": "eth_blockNumber", "params": [], "id": "getblock.io" }
response = requests.post(url, headers=headers, json=payload)
print(response.json())
```

#### JavaScript

For JavaScript examples, you'll need Node.js version 18 or later. Follow the official documentation to install the latest stable version globally. Verify your installation by running:

```
node -v
```

#### Ruby

To execute Ruby examples, install Ruby on your machine. Refer to the official installation guide for details. Confirm installation by running:

```
ruby -v
```

## Supported networks

We provide APIs for a wide range of networks, including:

[Aptos (APT) ](https://getblock.io/docs/apt/json-rpc/apt_v1_estimate_gas_price/)

[Arbitrum (ARB) ](https://getblock.io/docs/arb/json-rpc/arbitrum_eth_accounts/)

[Avalanche (AVAX) ](https://getblock.io/docs/avax/json-rpc/avax_eth_accounts/)

[Bitcoin Cash (BCH)](https://getblock.io/docs/bch/json-rpc/bch_combinepsbt/)&#x20;

[BNB Smart Chain (BSC) ](https://getblock.io/docs/bsc/json-rpc/bsc_debug_tracecall/)

[Bitcoin-SV (BSV) ](https://getblock.io/docs/bsv/json-rpc/bsv_analyzepsbt/)

[Bitcoin (BTC) ](https://getblock.io/docs/btc/json-rpc/btc_analyzepsbt/)

[Bitcoin Gold (BTG) ](https://getblock.io/docs/btg/json-rpc/btg_analyzepsbt/)

[Cronos (CRO) ](https://getblock.io/docs/cro/json-rpc/cro_eth_accounts/)

[Dash (DASH) ](https://getblock.io/docs/dash/json-rpc/dash_combinerawtransaction/)

[DigiByte (DGB) ](https://getblock.io/docs/dgb/json-rpc/dgb_analyzepsbt/)

[Dogeсoin (DOGE) ](https://getblock.io/docs/doge/json-rpc/doge_createrawtransaction/)

[Polkadot (DOT) ](https://getblock.io/docs/dot/json-rpc/dot_chain_getblock/)

[Ethereum Classic (ETC) ](https://getblock.io/docs/etc/json-rpc/etc_eth_accounts/)

[Ethereum (ETH)](https://getblock.io/docs/eth/json-rpc/eth_eth_accounts/)&#x20;

[Fantom (FTM) ](https://getblock.io/docs/ftm/json-rpc/ftm_dag_getevent/)

[Fuse.io (FUSE) ](https://getblock.io/docs/fuse/json-rpc/fuse_eth_accounts/)

[Moonbeam (GLMR) ](https://getblock.io/docs/glmr/json-rpc/glmr_eth_accounts/)

[Gnosis Chain (GNO) ](https://getblock.io/docs/gno/json-rpc/gno_eth_accounts/)

[Huobi ECO Chain (HECO) ](https://getblock.io/docs/heco/json-rpc/heco_eth_accounts/)

[KuCoin Community Chain (KCC) ](https://getblock.io/docs/kcc/json-rpc/kcc_eth_accounts/)

[Kusama (KSM) ](https://getblock.io/docs/ksm/json-rpc/ksm_chain_getblock/)

[Polygon (MATIC) ](https://getblock.io/docs/matic/json-rpc/matic_eth_accounts/)

[Moonriver (MOVR) ](https://getblock.io/docs/movr/json-rpc/movr_chain_getblock/)

[Near Protocol (NEAR) ](https://getblock.io/docs/near/json-rpc/near_block/)

[Harmony (ONE) ](https://getblock.io/docs/one/json-rpc/one_eth_accounts/)

[Ontology (ONT)](https://getblock.io/docs/ont/json-rpc/ont_getbalance/)&#x20;

[Optimism (OP)](https://getblock.io/docs/op/json-rpc/optimism_eth_accounts/)&#x20;

[Rootstock (RSK) ](https://getblock.io/docs/rsk/json-rpc/rsk_eth_accounts/)

[Solana (SOL) ](https://getblock.io/docs/sol/json-rpc/sol_getaccountinfo/)

[Theta (THETA) ](https://getblock.io/docs/theta/json-rpc/theta_theta.broadcastrawtransaction/)

[TON (TON) ](https://getblock.io/docs/ton/json-rpc/ton_detectaddress/)

[Tron (TRX) ](https://getblock.io/docs/trx/json-rpc/trx_eth_blocknumber/)

[NEM (XEM) ](https://getblock.io/docs/xem/rest/xem_account_get_forwarded_from-public-key/)

[Monero (XMR) ](https://getblock.io/docs/xmr/json-rpc/get_alternate_chains/)

[ZCash (ZEC)](https://getblock.io/docs/zec/json-rpc/zec_createrawtransaction/)
