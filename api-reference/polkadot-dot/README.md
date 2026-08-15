---
description: >-
  GetBlock provides fast and reliable access to Polkadot nodes via JSON-RPC API.
  Connect to the Polkadot network without running your own infrastructure.
---

# Polkadot (DOT)

Polkadot is a Layer 0 protocol that connects and secures a network of application-specific Layer 1 blockchains. It is a Substrate-based chain, so it does not use the EVM or the Ethereum JSON-RPC API. The native token is **DOT**, whose base unit is the Planck (1 DOT = 10^10 Planck). Consensus is Nominated Proof of Stake (NPoS), with BABE for block production and GRANDPA for finality.

GetBlock exposes Polkadot through three surfaces:

| Surface                                                                                       | Style        | Covers                                   | Use it for                                                                         |
| --------------------------------------------------------------------------------------------- | ------------ | ---------------------------------------- | ---------------------------------------------------------------------------------- |
| [Polkadot JSON-RPC API (Relaychain)](polkadot-json-rpc-api-relaychain/)                       | JSON-RPC 2.0 | Relay chain                              | Low-level node access: blocks, state, storage, fees, finality                      |
| [Substrate API Sidecar (AssetHub + Relaychain)](substrate-api-sidecar-assethub-+-relaychain/) | REST         | Asset Hub (default) + Relaychain (`/rc`) | High-level REST reads: accounts, balances, blocks, staking, pallets, transactions  |
| [Asset Hub JSON-RPC API (Primary Network)](asset-hub-json-rpc-api-primary-network.md)         | JSON-RPC 2.0 | Asset Hub system parachain               | Assets, foreign assets, and NFTs on Asset Hub, plus the standard Substrate methods |

The endpoint provides unified access to both Asset Hub and the Relaychain. For the REST (Sidecar) surface, **Asset Hub is the default** and the Relaychain is reached with an `/rc` path prefix. The JSON-RPC surfaces select their method by the request body, not the URL.

## Key Features

* **Substrate JSON-RPC**: Namespaced methods (`chain_`, `state_`, `system_`, `payment_`, `grandpa_`, and more) rather than an `eth_` API
* **NPoS Consensus**: Nominated Proof of Stake, with BABE block production and GRANDPA finality
* **Deterministic Finality**: GRANDPA finalizes blocks irreversibly; BEEFY adds bridge-friendly finality proofs
* **Runtime Metadata**: `state_getMetadata` returns SCALE-encoded metadata describing pallets, calls, and storage
* **Asset Hub**: The system parachain for assets and NFTs, positioned as the primary network for balances and tokens
* **Substrate API Sidecar**: A REST service over the node for high-level, decode-free reads
* **SS58 Addresses**: Polkadot uses SS58-encoded addresses (format 0), not `0x` hex addresses
* **Polkadot.js SDK**: The idiomatic JSON-RPC client is `@polkadot/api`, which connects over WebSocket

{% hint style="info" %}
GetBlock's Polkadot API reference documentation is provided for informational purposes and developer experience. Polkadot is a Substrate chain; the canonical specifications are maintained by the Polkadot and Substrate projects and published at [docs.polkadot.com](https://docs.polkadot.com/). Amounts are in Planck and addresses use the SS58 format.
{% endhint %}

## Network Information

| Property        | Value                                               |
| --------------- | --------------------------------------------------- |
| Network Name    | Polkadot                                            |
| Native Currency | DOT (1 DOT = 10^10 Planck)                          |
| Token Decimals  | 10                                                  |
| SS58 Format     | 0                                                   |
| Consensus       | NPoS with BABE (production) and GRANDPA (finality)  |
| API Styles      | Substrate JSON-RPC and Substrate API Sidecar (REST) |
| Address Format  | SS58-encoded                                        |
| Block Explorer  | [Subscan](https://polkadot.subscan.io/)             |
| SDK             | Polkadot.js API (`@polkadot/api`)                   |

## Base URL

{% tabs %}
{% tab title="HTTPS (JSON-RPC)" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="WebSocket (JSON-RPC)" %}
```bash
wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="REST (Sidecar)" %}
```bash
# Asset Hub (default)
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/blocks/head

# Relaychain (/rc prefix)
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rc/blocks/head
```
{% endtab %}
{% endtabs %}

Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard. JSON-RPC methods are called with a `POST` request and a JSON-RPC 2.0 body. Sidecar (REST) methods are called with `GET`/`POST` against path routes, where Asset Hub is the default and the Relaychain uses the `/rc` prefix.

## Supported Networks

| Network | JSON-RPC | WSS | Substrate | Mainnet-Asset-Hub | Testnet-Asset-hub | Frankfurt, Germany |
| ------- | -------- | --- | --------- | ----------------- | ----------------- | ------------------ |
| Mainnet | ✅        | ✅   | ✅         | ✅                 | ✅                 | ✅                  |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Polkadot Developer Documentation](https://docs.polkadot.com/)
* [Polkadot.js API](https://polkadot.js.org/docs/api/)
* [Substrate API Sidecar](https://github.com/paritytech/substrate-api-sidecar)
* [Subscan Explorer](https://polkadot.subscan.io/)
