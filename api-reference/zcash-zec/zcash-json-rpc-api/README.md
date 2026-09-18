---
description: >-
  GetBlock provides fast and reliable access to Zcash nodes via JSON-RPC API.
  Connect to the Zcash network without running your own infrastructure.
---

# Zcash JSON-RPC API

The Zcash JSON-RPC API exposes the Zebra (`zebrad`) node interface: a Bitcoin Core-compatible method set for blocks, transactions, the mempool, and mining, extended with `z_*` methods for the shielded pools. Requests `POST` a JSON-RPC 2.0 body to the endpoint; the method is selected by the body.

{% hint style="info" %}
Unlike most UTXO nodes, Zebra indexes **transparent** addresses, so `getaddressbalance`, `getaddresstxids`, and `getaddressutxos` answer address queries directly over JSON-RPC. Shielded (Sapling, Orchard) balances and notes cannot be enumerated by any public endpoint, because they are encrypted to the holder's viewing key.
{% endhint %}

### Base URL

```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```

### Request Format

{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockcount",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}



### Available Methods

#### Node & Client Info

| Method           | Description                                               |
| ---------------- | --------------------------------------------------------- |
| [`getinfo`](getinfo-zcash.md)        | Basic node status, version, block height, connections     |
| [`getnetworkinfo`](getnetworkinfo-zcash.md) | Network status, peer info, protocol version (Zebra 3.0.0) |

#### Blockchain State

| Method                      | Description                                               |
| --------------------------- | --------------------------------------------------------- |
| [`getblockchaininfo`](getblockchaininfo-zcash.md)         | Chain state, activated upgrades, value pool balances      |
| [`getbestblockhash`](getbestblockhash-zcash.md)          | Hash of the chain tip                                     |
| [`getbestblockheightandhash`](getbestblockheightandhash-zcash.md) | Chain tip height and hash together (Zebra-specific)       |
| [`getblockcount`](getblockcount-zcash.md)             | Chain tip height                                          |
| [`getblockhash`](getblockhash-zcash.md)              | Block hash by height                                      |
| [`getblock`](getblock-zcash.md)                  | Block by hash or height (verbosity 0/1/2, includes `nTx`) |
| [`getblockheader`](getblockheader-zcash.md)            | Block header only                                         |
| [`getdifficulty`](getdifficulty-zcash.md)             | Proof-of-work difficulty multiplier                       |

#### Transactions

| Method               | Description                             |
| -------------------- | --------------------------------------- |
| [`getrawtransaction`](getrawtransaction-zcash.md)  | Raw transaction by txid (verbosity 0/1) |
| [`sendrawtransaction`](sendrawtransaction-zcash.md) | Submit a signed raw transaction         |
| [`gettxout`](gettxout-zcash.md)           | UTXO lookup (transparent outputs only)  |

#### Mempool

| Method           | Description                      |
| ---------------- | -------------------------------- |
| [`getrawmempool`](getrawmempool-zcash.md)  | List transactions in the mempool |
| [`getmempoolinfo`](getmempoolinfo-zcash.md) | Mempool statistics (Zebra 3.0.0) |

#### Mining & Difficulty

| Method             | Description                                     |
| ------------------ | ----------------------------------------------- |
| [`getblocktemplate`](getblocktemplate-zcash.md) | Template for constructing candidate blocks      |
| [`getmininginfo`](getmininginfo-zcash.md)    | Mining status and current difficulty            |
| [`getnetworksolps`](getnetworksolps-zcash.md)  | Estimated network Equihash solutions per second |

#### Peer Info

| Method        | Description                                             |
| ------------- | ------------------------------------------------------- |
| [`getpeerinfo`](getpeerinfo-zcash.md) | Connected peer details (extended fields in Zebra 3.0.0) |

#### Address Queries (Zcash Extensions)

| Method              | Description                                   |
| ------------------- | --------------------------------------------- |
| [`getaddressbalance`](getaddressbalance-zcash.md) | Transparent balance for one or more addresses |
| [`getaddresstxids`](getaddresstxids-zcash.md)   | Transaction IDs involving an address          |
| [`getaddressutxos`](getaddressutxos-zcash.md)   | UTXOs held by an address                      |

#### Address & Script Utilities

| Method              | Description                                     |
| ------------------- | ----------------------------------------------- |
| [`validateaddress`](validateaddress-zcash.md)   | Validate a transparent (t1/t3) address          |
| [`z_validateaddress`](z_validateaddress-zcash.md) | Validate a Sapling, Orchard, or Unified address |

#### Shielded Pool

| Method                   | Description                                              |
| ------------------------ | -------------------------------------------------------- |
| [`z_getsubtreesbyindex`](z_getsubtreesbyindex-zcash.md)   | Subtree roots by index (Ironwood NCT)                    |
| [`z_gettreestate`](z_gettreestate-zcash.md) | Note commitment tree state at a given block |
| [`z_listunifiedreceivers`](z_listunifiedreceivers-zcash.md) | Decompose a Unified Address into its component receivers |

### Support

* Support: [support@getblock.io](mailto:support@getblock.io)
