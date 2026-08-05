---
description: >-
  GetBlock TRON API documentation — access TRON blockchain through REST, gRPC,
  JSON-RPC, and Solidity HTTP endpoints. RPC methods and guides for developers
---

# Tron (TRX)

TRON is a high-throughput Layer 1 blockchain that uses Delegated Proof of Stake (DPoS) consensus and the TRON Virtual Machine (TVM) for smart contracts. It is the leading network for stablecoin settlement, notably USDT (TRC-20). Instead of a single gas fee, TRON meters two resources — **Energy** for contract execution and **Bandwidth** for transaction size — which accounts obtain by staking TRX. The native token TRX is denominated in SUN (1 TRX = 1,000,000 SUN).

GetBlock exposes TRON through its native REST (HTTP) API and an Ethereum-compatible JSON-RPC API, over both node types.

## Interfaces

TRON nodes serve the same operation set through more than one transport and at two freshness levels:

| Interface   | Transport         | Node                                 | Use it for                                                                         |
| ----------- | ----------------- | ------------------------------------ | ---------------------------------------------------------------------------------- |
| REST (HTTP) | HTTP POST         | Fullnode (`/wallet`)                 | The native API: accounts, blocks, transactions, resources, contracts               |
| REST (HTTP) | HTTP POST         | Solidity (`/walletsolidity`)         | Confirmed, irreversible reads for balance and payment verification                 |
| JSON-RPC    | HTTP POST         | Fullnode (`/jsonrpc`)                | Ethereum-compatible `eth_*` methods for EVM-oriented tooling                       |
| gRPC        | HTTP/2 (protobuf) | Fullnode (`protocol.Wallet`)         | The native operation set over Protocol Buffers, for performance-sensitive backends |
| gRPC        | HTTP/2 (protobuf) | Solidity (`protocol.WalletSolidity`) | Confirmed, irreversible reads over gRPC                                            |

The **Fullnode** serves the complete API over the latest state, including transaction creation and broadcast. The **Solidity node** serves a query-only subset that returns only confirmed (irreversible) data. The REST and gRPC interfaces carry the same native operation set — the same request and response fields — over different transports; the JSON-RPC layer is a separate Ethereum-compatibility subset. TRON's idiomatic SDK, TronWeb, is built on the REST API.

## Key Features

* **DPoS Consensus**: 27 Super Representatives produce blocks with roughly 3-second block times
* **Energy and Bandwidth**: Two staked resources meter execution and transaction size instead of a single gas fee
* **Stablecoin Settlement**: The leading network for USDT (TRC-20) transfer volume
* **TVM Smart Contracts**: An EVM-compatible virtual machine running Solidity contracts as TRC-20 and TRC-721 tokens
* **TRC-10 and TRC-20 Tokens**: Native (TRC-10) and contract-based (TRC-20) token standards
* **Resource Delegation**: Stake 2.0 lets accounts delegate Energy and Bandwidth to others
* **Dual Node Types**: Fullnode for live state and Solidity node for confirmed data

{% hint style="info" %}
GetBlock's TRON API reference documentation is provided for informational purposes and developer experience. The canonical specification is maintained by the TRON protocol and published at [developers.tron.network](https://developers.tron.network/). Amounts are in SUN and addresses use TRON's base58 (T...) or hex (41...) formats.
{% endhint %}

## Network Information

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Network Name</td><td>TRON Mainnet</td></tr><tr><td>Native Currency</td><td>TRX (1 TRX = 1,000,000 SUN)</td></tr><tr><td>JSON-RPC Chain ID</td><td>728126428 (<code>0x2b6653dc</code>)</td></tr><tr><td>Consensus</td><td>Delegated Proof of Stake (DPoS)</td></tr><tr><td>Block Time</td><td>~3 seconds</td></tr><tr><td>Smart Contract VM</td><td>TVM (TRON Virtual Machine, EVM-compatible)</td></tr><tr><td>Resources</td><td>Energy (execution) and Bandwidth (size)</td></tr><tr><td>Address Formats</td><td>base58 (<code>T...</code>) and hex (<code>41...</code>)</td></tr><tr><td>Block Explorer</td><td><a href="https://tronscan.org/">tronscan.org</a></td></tr></tbody></table>

## Base URL

{% tabs %}
{% tab title="REST (Fullnode)" %}
```bash
https://go.getblock.io/<ACCESS-TOKEN>/wallet/{method}
```
{% endtab %}

{% tab title="REST (Solidity)" %}
```bash
https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/{method}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```bash
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="gRPC" %}
```bash
go.getblock.io:443   (TLS, access token passed as request metadata)
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard. The REST and JSON-RPC interfaces are HTTP `POST`; gRPC connects to a `host:port` target over TLS. Confirm the exact gRPC target and access-token mechanism from the dashboard when provisioning a gRPC endpoint.
{% endhint %}

## Supported Networks

| Network | REST(Fullnode) | JSON-RPC(Fullnode) | REST(Solidity) | JSON-RPC(Solidty) | gRPC(Fullnode) | gRPC(Solidity) | Frankfurt,Germany | New York, USA | Singapore, Singapore |
| ------- | -------------- | ------------------ | -------------- | ----------------- | -------------- | -------------- | ----------------- | ------------- | -------------------- |
| Mainnet | ✅              | ✅                  | ✅              | ✅                 | ✅              | ✅              | ✅                 | ✅             | ✅                    |
| Testnet | ✅              | ✅                  | ✅              | ✅                 | ✅              | ✅              | ✅                 | ❌             | ❌                    |

## APIs

* [REST API](tron-rest-api/) — the native TRON HTTP API
* [JSON-RPC API](tron-json-rpc-api/) — the Ethereum-compatible subset
* [gRPC](tron-grpc-api/) — the native operation set over Protocol Buffers (`protocol.Wallet` and `protocol.WalletSolidity`)

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [TRON Developer Documentation](https://developers.tron.network/)
* [TronWeb SDK](https://tronweb.network/)
* [TRONSCAN Explorer](https://tronscan.org/)
