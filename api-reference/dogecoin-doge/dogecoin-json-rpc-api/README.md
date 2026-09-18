---
description: >-
  GetBlock provides fast and reliable access to Dogecoin nodes via JSON-RPC API.
  Connect to the Dogecoin network without running your own infrastructure.
---

# Dogecoin JSON-RPC API

The Dogecoin JSON-RPC API exposes the Dogecoin Core node interface: methods for reading blocks and transactions, inspecting individual unspent outputs, building and signing raw transactions, and reading node and mining state. Requests `POST` a JSON-RPC 2.0 body to the endpoint; the method is selected by the body.

{% hint style="info" %}
This is the node interface. It has no address index, so it cannot answer "what is the balance of this address" or "what are the unspent outputs of this address". Those queries belong on the Blockbook add-on, which is provisioned as its own endpoint.
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

#### Blockchain information

| Method                                                 | Description                                                     |
| ------------------------------------------------------ | ----------------------------------------------------------------- |
| [`getblockchaininfo`](getblockchaininfo-dogecoin.md)   | Returns state information about blockchain processing           |
| [`getbestblockhash`](getbestblockhash-dogecoin.md)     | Returns the hash of the current chain tip                       |
| [`getblockcount`](getblockcount-dogecoin.md)           | Returns the number of blocks in the longest blockchain          |
| [`getdifficulty`](getdifficulty-dogecoin.md)           | Returns the current mining difficulty                           |
| [`getchaintips`](getchaintips-dogecoin.md)             | Returns information about all known tips in the block tree      |
| [`getinfo`](getinfo-dogecoin.md)                       | Returns an object containing various state info about the node  |
| [`getconnectioncount`](getconnectioncount-dogecoin.md) | Returns the number of connections to other nodes                |
| [`getmemoryinfo`](getmemoryinfo-dogecoin.md)           | Returns information about the node's memory usage               |
| [`help`](help-dogecoin.md)                             | Lists every command the node exposes, or describes one          |

#### Block retrieval

| Method                                             | Description                                                |
| -------------------------------------------------- | ------------------------------------------------------------ |
| [`getblock`](getblock-dogecoin.md)                 | Returns block data for a given block hash                  |
| [`getblockhash`](getblockhash-dogecoin.md)         | Returns the hash of the block at a given height            |
| [`getblockheader`](getblockheader-dogecoin.md)     | Returns a block header without its transactions            |

#### Transaction methods

| Method                                                     | Description                                                   |
| ---------------------------------------------------------- | --------------------------------------------------------------- |
| [`getrawtransaction`](getrawtransaction-dogecoin.md)       | Returns the raw transaction data for a given transaction id   |
| [`decoderawtransaction`](decoderawtransaction-dogecoin.md) | Decodes a hex-encoded raw transaction and returns it as JSON  |
| [`createrawtransaction`](createrawtransaction-dogecoin.md) | Creates a raw transaction spending the given inputs           |
| [`signrawtransaction`](signrawtransaction-dogecoin.md)     | Signs inputs for a serialized, hex-encoded raw transaction    |
| [`sendrawtransaction`](sendrawtransaction-dogecoin.md)     | Broadcasts a signed transaction and returns its id            |
| [`decodescript`](decodescript-dogecoin.md)                 | Decodes a hex-encoded script                                  |

#### Mempool methods

| Method                                                       | Description                                                     |
| ------------------------------------------------------------ | ----------------------------------------------------------------- |
| [`getrawmempool`](getrawmempool-dogecoin.md)                 | Returns the transaction ids of everything in the memory pool    |
| [`getmempoolinfo`](getmempoolinfo-dogecoin.md)               | Returns details on the active state of the memory pool          |
| [`getmempoolentry`](getmempoolentry-dogecoin.md)             | Returns mempool data for a single queued transaction            |
| [`getmempoolancestors`](getmempoolancestors-dogecoin.md)     | Returns the unconfirmed transactions a transaction depends on   |
| [`getmempooldescendants`](getmempooldescendants-dogecoin.md) | Returns the unconfirmed transactions that depend on one         |

#### UTXO and proofs

| Method                                         | Description                                                      |
| ---------------------------------------------- | ------------------------------------------------------------------ |
| [`gettxout`](gettxout-dogecoin.md)             | Returns details about a single unspent transaction output        |
| [`gettxoutproof`](gettxoutproof-dogecoin.md)   | Returns a merkle proof that transactions were included in a block |

#### Mining methods

| Method                                             | Description                                             |
| -------------------------------------------------- | --------------------------------------------------------- |
| [`getmininginfo`](getmininginfo-dogecoin.md)       | Returns mining-related information                      |
| [`getnetworkhashps`](getnetworkhashps-dogecoin.md) | Returns the estimated network hashes per second         |
| [`getblocktemplate`](getblocktemplate-dogecoin.md) | Returns the data needed to construct a candidate block  |

#### Utility methods

| Method                                           | Description                                          |
| ------------------------------------------------ | ---------------------------------------------------- |
| [`validateaddress`](validateaddress-dogecoin.md) | Validates a Dogecoin address and returns information |
| [`verifymessage`](verifymessage-dogecoin.md)     | Verifies a signed message                            |

{% hint style="info" %}
**Dogecoin Core is based on an older Bitcoin Core release**, so methods added to Bitcoin Core later are not available here. The PSBT family (`createpsbt`, `decodepsbt`, `combinepsbt`, `finalizepsbt`, `converttopsbt`), `signrawtransactionwithkey`, `getblockstats`, `testmempoolaccept`, and `getrpcinfo` are absent; use `createrawtransaction` and `signrawtransaction` in place of the PSBT workflow. Methods specific to other forks, such as Bitcoin Cash Node's `getfinalizedblockhash`, do not exist on Dogecoin either.

Call [`help`](help-dogecoin.md) with no parameter to get the authoritative list from the node itself.
{% endhint %}

### Wallet methods are not available

Dogecoin Core's **wallet** RPC methods — `listunspent`, `listtransactions`, `getbalance`, `sendtoaddress`, `getnewaddress`, `dumpprivkey`, `importprivkey`, `walletpassphrase`, and the rest — operate on a wallet loaded on the node itself. Shared nodes carry no user wallets, so these methods are disabled and are not documented here.

Address-level questions are answered by the Blockbook add-on instead:

| Dogecoin Core wallet method | Blockbook REST equivalent                 |
| --------------------------- | ----------------------------------------- |
| `listunspent`               | `/api/v2/utxo/{address}`                  |
| `listtransactions`          | `/api/v2/address/{address}?details=txs`   |
| `getbalance`                | `/api/v2/address/{address}?details=basic` |
| `getreceivedbyaddress`      | `/api/v2/address/{address}?details=basic` |
| `gettransaction`            | `/api/v2/tx/{txid}`                       |

{% hint style="warning" %}
`gettxout` reports a single outpoint that the caller already knows. It cannot list the outputs belonging to an address, which is the query most integrations actually need. Use the Blockbook UTXO endpoint for that.
{% endhint %}

### Support

* Support: [support@getblock.io](mailto:support@getblock.io)
