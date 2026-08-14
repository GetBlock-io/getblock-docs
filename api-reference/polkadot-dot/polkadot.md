# polkadot

Polkadot is a Layer 0 relay chain that connects and secures a network of application-specific Layer 1 blockchains called parachains. It is a Substrate-based chain, so it does not use the EVM or the Ethereum JSON-RPC API. Instead, it exposes the Substrate JSON-RPC interface, organized into namespaces such as `chain_`, `state_`, `system_`, and `grandpa_`. The native token is **DOT**, whose base unit is the Planck (1 DOT = 10^10 Planck).

Polkadot secures the network with Nominated Proof of Stake (NPoS), using BABE for block production and GRANDPA for finality. This documentation covers the read-only Substrate JSON-RPC methods available through GetBlock.

## Key Features

* **Substrate JSON-RPC**: Namespaced methods (`chain_`, `state_`, `system_`, `grandpa_`, `rpc_`) rather than an `eth_` API
* **NPoS Consensus**: Nominated Proof of Stake, with BABE block production and GRANDPA finality
* **Deterministic Finality**: GRANDPA finalizes blocks irreversibly, exposed via `chain_getFinalizedHead`
* **Runtime Metadata**: `state_getMetadata` returns SCALE-encoded metadata describing pallets, calls, and storage
* **Forkless Upgrades**: The runtime version is read via `state_getRuntimeVersion` and can be upgraded on-chain
* **SS58 Addresses**: Polkadot uses SS58-encoded addresses (format 0), not `0x` hex addresses
* **Polkadot.js SDK**: The idiomatic client is `@polkadot/api`, which connects over WebSocket

{% hint style="info" %}
GetBlock's Polkadot API reference documentation is provided for informational purposes and developer experience. Polkadot is a Substrate chain; the canonical specification is maintained by the Polkadot and Substrate projects and published at [docs.polkadot.com](https://docs.polkadot.com/). Amounts are in Planck and addresses use the SS58 format.
{% endhint %}

## Network Information

| Property        | Value                                              |
| --------------- | -------------------------------------------------- |
| Network Name    | Polkadot                                           |
| Native Currency | DOT (1 DOT = 10^10 Planck)                         |
| Token Decimals  | 10                                                 |
| SS58 Format     | 0                                                  |
| Consensus       | NPoS with BABE (production) and GRANDPA (finality) |
| API Style       | Substrate JSON-RPC (no EVM, no `eth_` namespace)   |
| Address Format  | SS58-encoded                                       |
| Block Explorer  | [Subscan](https://polkadot.subscan.io/)            |
| SDK             | Polkadot.js API (`@polkadot/api`)                  |

## Base URL

{% tabs %}
{% tab title="HTTPS" %}
```bash
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="WebSocket" %}
```bash
wss://go.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

Methods are called by sending a `POST` request with a JSON-RPC 2.0 body to the HTTPS endpoint. The Polkadot.js API connects over the WebSocket endpoint. Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard.

## Supported Networks

| Network | JSON-RPC | WSS | Frankfurt, Germany |
| ------- | -------- | --- | ------------------ |
| Mainnet | ✅        | ✅   | ✅                  |

## Available API Methods

GetBlock exposes the read-only Substrate JSON-RPC methods below. Write and subscription methods (`author_*`, `*_subscribe*`), and node-administration methods, are not available on shared endpoints.

### Chain

| Method                                                                            | Description                               |
| --------------------------------------------------------------------------------- | ----------------------------------------- |
| [chain\_getBlock](/broken/pages/3cb46f3bab9cee36a6442e84dbf8a9421739b3cc)         | Returns a full block by hash              |
| [chain\_getBlockHash](/broken/pages/483140cfe00b19f07ac39c7faabf68c151c2d4ff)     | Returns the block hash for a block number |
| [chain\_getFinalizedHead](/broken/pages/54c2faa60bf68a1e4c23e5932d7a84b0eccd943a) | Returns the latest finalized block hash   |
| [chain\_getHeader](/broken/pages/0a823d35bb047f3074f4ddd04895fd1cef44bf8b)        | Returns a block header by hash            |

### State

| Method                                                                             | Description                            |
| ---------------------------------------------------------------------------------- | -------------------------------------- |
| [state\_getRuntimeVersion](/broken/pages/71ca74a8c3e88991f7779db866956671726e2d64) | Returns the runtime version at a block |
| [state\_getMetadata](/broken/pages/2e0a979ef358ca3088573601800e8cc7ae76dbd5)       | Returns SCALE-encoded runtime metadata |

### System

| Method                                                                       | Description                                     |
| ---------------------------------------------------------------------------- | ----------------------------------------------- |
| [system\_chain](/broken/pages/15678b51030a12ca54077f2bb6bb33bf67154881)      | Returns the chain name                          |
| [system\_chainType](/broken/pages/f81f3dbe62bfbd55a85532824401008df7677f3d)  | Returns the chain type (Live, Local, and so on) |
| [system\_health](/broken/pages/f9ac8260d7f165eddfdd08d420d913363c17cb72)     | Returns node health and peer count              |
| [system\_properties](/broken/pages/6f1c98ee83e0f23d7afff0866ebe6360bfff2e72) | Returns token symbol, decimals, and SS58 format |
| [system\_syncState](/broken/pages/f864629c0c92c4bd0456a1395f84064cb4abd1ce)  | Returns the node's sync progress                |
| [system\_version](/broken/pages/80965abc77b710af9da857912897ad0a6df87b30)    | Returns the client software version             |

### Consensus & Discovery

| Method                                                                        | Description                                  |
| ----------------------------------------------------------------------------- | -------------------------------------------- |
| [grandpa\_roundState](/broken/pages/a6494f515459647fe46bc1d49a15b62af9c9bc48) | Returns the current GRANDPA round state      |
| [rpc\_methods](/broken/pages/b950b8ecdc887148fbf443666c0ae1a62a0f08f9)        | Returns the list of exposed JSON-RPC methods |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Polkadot Developer Documentation](https://docs.polkadot.com/)
* [Polkadot.js API](https://polkadot.js.org/docs/api/)
* [Subscan Explorer](https://polkadot.subscan.io/)
