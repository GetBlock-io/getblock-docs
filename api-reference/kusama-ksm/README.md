---
tags:
  - kusama
---

# Kusama (KSM)

Kusama is a Substrate-based, Nominated-Proof-of-Stake relay chain — Polkadot's canary network — used to trial runtime upgrades and parachains under real economic conditions before they reach Polkadot. It is not an EVM chain: applications interact with it through the Substrate JSON-RPC, a set of namespaced `module_method` calls (for example `chain_getBlock`, `state_getStorage`, `system_health`). The native token is KSM, denominated in Planck (10^-12 KSM), and accounts use the SS58 address format with Kusama's prefix (2).

## Interfaces

| Interface       | Transport                             | Use it for                                                                                  |
| --------------- | ------------------------------------- | ------------------------------------------------------------------------------------------- |
| JSON-RPC (HTTP) | Substrate JSON-RPC 2.0 over HTTP POST | Request/response queries and extrinsic submission                                           |
| Substrate       | The Substrate JSON-RPC method family  | The same namespaced `module_method` calls (served over both HTTP and WebSocket)             |
| WebSocket (WSS) | Substrate JSON-RPC 2.0 over WebSocket | The same methods, plus pub-sub subscriptions (new heads, storage changes, extrinsic status) |

The JSON-RPC and WebSocket interfaces share the same Substrate method set; "Substrate" refers to that method family. HTTP is best for one-off request/response calls, while WebSocket keeps a persistent connection and is required for subscriptions.

* [Substrate JSON-RPC API ](substrate-json-rpc-api-kusama/)— request/response, HTTP + WS
* [WebSocket Subscriptions](websocket-subscriptions-api-kusama/) — WS-only streams

## Interface Endpoints

{% tabs %}
{% tab title="JSON-RPC (HTTP)" %}
{% code overflow="wrap" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endcode %}
{% endtab %}

{% tab title="WebSocket (WSS)" %}
```bash
wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Replace `<ACCESS-TOKEN>` with the token from the GetBlock dashboard. Each interface is provisioned as its own endpoint; the WebSocket interface is required for subscriptions.
{% endhint %}

## Network Information

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Network Name</td><td>Kusama</td></tr><tr><td>Native Currency</td><td>KSM</td></tr><tr><td>Token Decimals</td><td>12 (Planck = 10^-12 KSM)</td></tr><tr><td>SS58 Format</td><td>2</td></tr><tr><td>Framework</td><td>Substrate (relay chain)</td></tr><tr><td>Consensus</td><td>Nominated Proof-of-Stake (BABE + GRANDPA)</td></tr><tr><td>Block Time</td><td>~6 seconds</td></tr><tr><td>Finality</td><td>Deterministic (GRANDPA)</td></tr></tbody></table>

## Supported Networks

| Network | JSON-RPC (HTTP) | WebSocket (WSS) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | --------------- | --------------- | ------------------ | ------------- | -------------------- |
| Mainnet | ✅               | ✅               | ✅                  | ❌             | ❌                    |

## APIs

* [Substrate JSON-RPC API ](substrate-json-rpc-api-kusama/)— request/response methods (System, Chain, State, Author, Payment, RPC), over HTTP and WebSocket
* [WebSocket Subscriptions](websocket-subscriptions-api-kusama/) — pub-sub streams (new heads, finalized heads, storage, extrinsic status)

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Polkadot Wiki](https://wiki.polkadot.network/)
* [Substrate JSON-RPC](https://guide.kusama.network/docs/build-node-interaction)
* [Polkadot-JS API](https://polkadot.js.org/docs/)
* [Subscan Explorer (Kusama)](https://kusama.subscan.io/)
