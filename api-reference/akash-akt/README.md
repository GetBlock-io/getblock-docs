---
description: >-
  GetBlock provides fast and reliable access to Akash nodes via JSON-RPC API.
  Connect to the Akash network without running your own infrastructure.
---

# Akash (AKT)

Akash Network is a decentralized cloud-compute marketplace built on the Cosmos SDK and secured by CometBFT (Tendermint) consensus for fast, deterministic finality. Rather than a single provider, Akash matches users who need compute with a marketplace of providers: a user posts a deployment describing the resources they want, providers submit bids, and an accepted bid becomes a lease that runs the workload — all settled on-chain in the native AKT token and funded through escrow. Because Akash is a Cosmos SDK chain, GetBlock exposes it over two interfaces: JSON-RPC (the CometBFT node RPC) and REST (the Cosmos SDK LCD, including Akash's own marketplace modules).

### Key Features

* **Decentralized Compute Marketplace**: Deployments, orders, bids, leases, and providers coordinate cloud compute on-chain
* **Cosmos SDK + CometBFT**: Deterministic single-block finality and IBC connectivity
* **AKT Settlement**: Leases are priced and settled in AKT, funded through per-deployment escrow accounts
* **Akash Modules**: Chain-specific `deployment`, `market`, and `provider` modules exposed over REST
* **Two Interfaces**: CometBFT JSON-RPC for consensus/block/tx data and broadcast; Cosmos REST for module queries
* **bech32 Addresses**: Accounts use the `akash1…` address format

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE API SPECIFICATION._

_GetBlock's API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical specifications are the CometBFT RPC (for JSON-RPC), the Cosmos SDK REST/gRPC (for standard modules), and the Akash Network protobuf definitions (for the deployment, market, and provider modules), published at_ [_docs.akash.network_](https://docs.akash.network/)_._
{% endhint %}

### Network Information

| Property        | Value                        |
| --------------- | ---------------------------- |
| Network Name    | Akash (akashnet-2)           |
| Native Currency | AKT (1 AKT = 1,000,000 uakt) |
| Consensus       | CometBFT (Tendermint) BFT    |
| Framework       | Cosmos SDK                   |
| Address Format  | bech32 (akash1…)             |
| Block Time      | \~6 seconds                  |
| Finality        | Deterministic (single block) |

### Interfaces

| Interface | Transport                       | Use it for                                                                                                  |
| --------- | ------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| JSON-RPC  | CometBFT JSON-RPC 2.0 over HTTP | Consensus, block, and transaction data; transaction broadcast; `abci_query` for any module state            |
| REST      | Cosmos SDK REST (LCD) over HTTP | Module queries — bank, staking, gov, distribution, and Akash's `deployment` / `market` / `provider` modules |

### Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

### Supported Networks

| Network              | JSON-RPC | REST | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| -------------------- | -------- | ---- | ------------------ | ------------- | -------------------- |
| Mainnet (akashnet-2) | ✅        | ✅    | ✅                  | ❌             | ❌                    |

### APIs

* [JSON-RPC API](json_rpc-api/) — CometBFT consensus, block, tx, and `abci_query`
* [REST API](rest-api/) — Cosmos SDK and Akash marketplace module queries

### Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

#### See Also

* [Akash Documentation](https://docs.akash.network/)
* [CometBFT RPC](https://docs.cometbft.com/main/rpc/)
* [Cosmos SDK REST](https://docs.cosmos.network/)
* [Akash Block Explorer (Mintscan)](https://www.mintscan.io/akash)
