---
description: >-
  GetBlock provides fast and reliable access to Polkadot nodes via the JSON-RPC
  API. Connect to the Polkadot network without running your own infrastructure.
---

# Polkadot (DOT)

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
_GetBlock's Polkadot API reference documentation is provided for informational purposes and developer experience. Polkadot is a Substrate chain; the canonical specification is maintained by the Polkadot and Substrate projects and published at_ [_docs.polkadot.com_](https://docs.polkadot.com/)_. Amounts are in Planck and addresses use the SS58 format._
{% endhint %}

## Network Information

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Network Name</td><td>Polkadot</td></tr><tr><td>Native Currency</td><td>DOT (1 DOT = 10^10 Planck)</td></tr><tr><td>Token Decimals</td><td>10</td></tr><tr><td>SS58 Format</td><td>0</td></tr><tr><td>Consensus</td><td>NPoS with BABE (production) and GRANDPA (finality)</td></tr><tr><td>API Style</td><td>Substrate JSON-RPC (no EVM, no <code>eth_</code> namespace)</td></tr><tr><td>Address Format</td><td>SS58-encoded</td></tr><tr><td>Block Explorer</td><td><a href="https://polkadot.subscan.io/">Subscan</a></td></tr><tr><td>SDK</td><td>Polkadot.js API (<code>@polkadot/api</code>)</td></tr></tbody></table>

## Base URL

{% tabs %}
{% tab title="HTTPS" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>
```
{% endtab %}

{% tab title="WebSocket" %}
```bash
wss://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Methods are called by sending a `POST` request with a JSON-RPC 2.0 body to the HTTPS endpoint. The Polkadot.js API connects over the WebSocket endpoint. Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard.
{% endhint %}

## Supported Networks

| Network | JSON-RPC | WSS | Substrate | Mainnet-Asset-Hub | Testnet-Asset-hub | Frankfurt, Germany |
| ------- | -------- | --- | --------- | ----------------- | ----------------- | ------------------ |
| Mainnet | ✅        | ✅   | ✅         | ✅                 | ✅                 | ✅                  |

## Available API Methods

GetBlock exposes the read-only Substrate JSON-RPC methods below. Write and subscription methods (`author_*`, `*_subscribe*`), and node-administration methods, are not available on shared endpoints.

### Chain

| Method                  | Description                               |
| ----------------------- | ----------------------------------------- |
| chain\_getBlock         | Returns a full block by hash              |
| chain\_getBlockHash     | Returns the block hash for a block number |
| chain\_getFinalizedHead | Returns the latest finalized block hash   |
| chain\_getHeader        | Returns a block header by hash            |

### State

| Method                   | Description                            |
| ------------------------ | -------------------------------------- |
| state\_getRuntimeVersion | Returns the runtime version at a block |
| state\_getMetadata       | Returns SCALE-encoded runtime metadata |

### System

| Method             | Description                                     |
| ------------------ | ----------------------------------------------- |
| system\_chain      | Returns the chain name                          |
| system\_chainType  | Returns the chain type (Live, Local, and so on) |
| system\_health     | Returns node health and peer count              |
| system\_properties | Returns token symbol, decimals, and SS58 format |
| system\_syncState  | Returns the node's sync progress                |
| system\_version    | Returns the client software version             |

### Consensus & Discovery

| Method              | Description                                  |
| ------------------- | -------------------------------------------- |
| grandpa\_roundState | Returns the current GRANDPA round state      |
| rpc\_methods        | Returns the list of exposed JSON-RPC methods |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Polkadot Developer Documentation](https://docs.polkadot.com/)
* [Polkadot.js API](https://polkadot.js.org/docs/api/)
* [Subscan Explorer](https://polkadot.subscan.io/)
