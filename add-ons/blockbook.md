---
description: >-
  GetBlock Blockbook: an address and wallet indexer for UTXO chains such as
  Bitcoin.
---

# Blockbook

Blockbook is an indexer for UTXO chains. It answers questions about an address or a whole wallet — balances, transaction history, and unspent outputs — that a standard Bitcoin node cannot answer on its own.

A Bitcoin node tracks the set of unspent transaction outputs (UTXOs), but it does not organize them by address. Ask a plain node for the balance of an address and it has no direct way to reply, because it was never built to look up history that way. Anyone who needs address-level data has to build a separate index over the chain.

Blockbook, built by Trezor, is that index. It reads the chain and maintains an address-indexed and xpub-indexed view on top of it. You query an address or a wallet, and Blockbook returns the answer from its index.

```mermaid
flowchart LR
    BC[UTXO chain] --> BB["Blockbook index<br/>by address and xpub"]
    BB --> Q["Your query:<br/>balance · transactions · UTXOs"]
```

### What it does

* **Address queries:** Ask for the balance, transaction history, or unspent outputs for a single address.
* **Wallet queries:** Request a full wallet at once using an xpub or an output descriptor. Blockbook derives the addresses and returns the combined result, so you do not track each address yourself.
* **Derivation support:** The add-on covers the BIP44, BIP49, BIP84, and BIP86 (Taproot) schemes, so it reads legacy, SegWit, native SegWit, and Taproot wallets.
* **Filtering and paging:** Page through large result sets and filter transactions by block-height range.

An xpub query, for instance, returns the balance for the entire wallet, the derived addresses, and the transactions, sorted by block height with the newest first.

### Supported networks

Because Blockbook indexes unspent outputs by address, the configurator offers it only on Bitcoin-type chains:

| Chain        | Ticker |
| ------------ | ------ |
| Bitcoin      | BTC    |
| Bitcoin Cash | BCH    |
| Dash         | DASH   |
| Dogecoin     | DOGE   |
| Zcash        | ZEC    |

Each chain's paths and methods are listed under its own entry in the API reference. The schema is shared across all of them, so a query written for one chain works on another with only the endpoint changed.

{% hint style="info" %}
**REST and WebSocket are provisioned separately.** Blockbook exposes the same index through two interfaces: a REST API for one-off queries, and a WebSocket API that also supports subscriptions. Each is its own endpoint with its own URL, so enabling Blockbook (REST) does not give you Blockbook (WS). A project that needs to watch an address as well as query it needs both.
{% endhint %}

### Benefits

* **Wallet-level answers:** One xpub call returns a full wallet, instead of many per-address calls.
* **No indexer to build:** You skip building and operating an address index next to your node.
* **Direct UTXO access:** You read unspent outputs to construct a spending transaction.
* **Broad wallet coverage:** Legacy through Taproot wallets are all supported.

### When to use it

* You build a Bitcoin wallet or a portfolio tracker.
* You need address balances or transaction history on a UTXO chain.
* You construct transactions and need the UTXOs behind an address.

### Limitations

* Blockbook serves UTXO (Bitcoin-type) chains. It does not apply to account-based chains such as Ethereum.
* The interface serves many coins through a shared schema, so some fields apply only to specific chains.
