---
description: >-
  GetBlock provides fast and reliable access to Litecoin nodes via JSON-RPC API.
  Connect to the Litecoin network without running your own infrastructure.
---

# Litecoin JSON-RPC API

The Litecoin JSON-RPC API exposes the Litecoin Core node interface: methods for reading blocks, transactions, and UTXOs, inspecting the mempool, building and broadcasting raw transactions, and estimating fees. Requests `POST` a JSON-RPC 2.0 body to the endpoint; the method is selected by the body.

{% hint style="info" %}
This is the node interface. It has no address index, so it cannot answer "what is the balance of this address" or "what are the unspent outputs of this address". Those queries belong on the [Blockbook REST](../litecoin-blockbook-rest-api/) or [Blockbook WebSocket](../litecoin-blockbook-websocket-api/) add-on, which is provisioned as its own endpoint.
{% endhint %}

### Base URL

```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```

### Request Format

Every request is a `POST` carrying a JSON-RPC 2.0 body:

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

Methods that require wallet or node-administration privileges are not available on shared endpoints and are omitted below.

#### Blockchain information

Methods that report the state of the chain.

| Method                                               | Description                                                                     |
| ---------------------------------------------------- | ------------------------------------------------------------------------------- |
| [`getblockchaininfo`](getblockchaininfo-litecoin.md) | Returns an object containing various state info regarding blockchain processing |
| [`getbestblockhash`](getbestblockhash-litecoin.md)   | Returns the hash of the best (tip) block in the longest blockchain              |
| [`getblockcount`](getblockcount-litecoin.md)         | Returns the number of blocks in the longest blockchain                          |
| [`getdifficulty`](getdifficulty-litecoin.md)         | Returns the proof-of-work difficulty as a multiple of the minimum difficulty    |
| [`getchaintips`](getchaintips-litecoin.md)           | Returns information about all known tips in the block tree                      |

#### Block retrieval

Methods that fetch block data.

| Method                                             | Description                                                         |
| -------------------------------------------------- | ------------------------------------------------------------------- |
| [`getblock`](getblock-litecoin.md)                 | Returns an object with information about the block for a given hash |
| [`getblockhash`](getblockhash-litecoin.md)         | Returns the hash of the block at the given height                   |
| [`getblockstats`](getblockstats-litecoin.md)       | Computes per-block statistics for a given window                    |
| [`getblocktemplate`](getblocktemplate-litecoin.md) | Returns data needed to construct a block to work on                 |

#### Transaction methods

Methods for reading, creating, decoding, and testing transactions.

| Method                                                     | Description                                                        |
| ---------------------------------------------------------- | ------------------------------------------------------------------ |
| [`getrawtransaction`](getrawtransaction-litecoin.md)       | Returns the raw transaction data                                   |
| [`decoderawtransaction`](decoderawtransaction-litecoin.md) | Returns a JSON object representing the serialized transaction      |
| [`decodescript`](decodescript-litecoin.md)                 | Decodes a hex-encoded script                                       |
| [`createrawtransaction`](createrawtransaction-litecoin.md) | Creates a transaction spending the given inputs                    |
| [`createmultisig`](createmultisig-litecoin.md)             | Creates a multi-signature address from a set of public keys        |
| [`testmempoolaccept`](testmempoolaccept-litecoin.md)       | Tests whether a raw transaction would be accepted into the mempool |

#### Mempool methods

Methods for querying the pool of unconfirmed transactions.

| Method                                                       | Description                                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------------ |
| [`getmempoolinfo`](getmempoolinfo-litecoin.md)               | Returns details on the active state of the transaction memory pool |
| [`getmempoolentry`](getmempoolentry-litecoin.md)             | Returns mempool data for a given transaction                       |
| [`getmempoolancestors`](getmempoolancestors-litecoin.md)     | Returns all in-mempool ancestors of a transaction                  |
| [`getmempooldescendants`](getmempooldescendants-litecoin.md) | Returns all in-mempool descendants of a transaction                |

#### Mining methods

Methods related to mining operations and statistics.

| Method                                             | Description                                                 |
| -------------------------------------------------- | ----------------------------------------------------------- |
| [`getmininginfo`](getmininginfo-litecoin.md)       | Returns a JSON object containing mining-related information |
| [`getnetworkhashps`](getnetworkhashps-litecoin.md) | Returns the estimated network hashes per second             |

#### Network methods

Methods for querying network state and peer information.

| Method                                                 | Description                                        |
| ------------------------------------------------------ | -------------------------------------------------- |
| [`getconnectioncount`](getconnectioncount-litecoin.md) | Returns the number of connections to other nodes   |
| [`getnettotals`](getnettotals-litecoin.md)             | Returns information about network traffic          |
| [`listbanned`](listbanned-litecoin.md)                 | Lists all manually banned IP addresses and subnets |
| [`getmemoryinfo`](getmemoryinfo-litecoin.md)           | Returns information about memory usage             |

#### UTXO methods

Methods for querying unspent transaction outputs by outpoint.

| Method                                           | Description                                                     |
| ------------------------------------------------ | --------------------------------------------------------------- |
| [`gettxout`](gettxout-litecoin.md)               | Returns details about an unspent transaction output             |
| [`gettxoutsetinfo`](gettxoutsetinfo-litecoin.md) | Returns statistics about the unspent transaction output set     |
| [`gettxoutproof`](gettxoutproof-litecoin.md)     | Returns a hex-encoded proof that txids were included in a block |

#### Utility methods

General validation and information methods.

| Method                                             | Description                                              |
| -------------------------------------------------- | -------------------------------------------------------- |
| [`validateaddress`](validateaddress-litecoin.md)   | Returns information about the given Litecoin address     |
| [`verifymessage`](verifymessage-litecoin.md)       | Verifies a signed message                                |
| [`estimatesmartfee`](estimatesmartfee-litecoin.md) | Estimates the approximate fee per kilobyte               |
| [`help`](help-litecoin.md)                         | Lists all commands, or gets help for a specified command |

{% hint style="warning" %}
`gettxout` cannot list the outputs belonging to an address. For that, use [api/v2/utxo](../litecoin-blockbook-rest-api/api-v2-utxo-litecoin.md) on the Blockbook add-on. The Litecoin Core wallet RPCs, including `listunspent`, are disabled on shared nodes because those nodes carry no user wallets.
{% endhint %}

### Support

* Support: [support@getblock.io](mailto:support@getblock.io)
