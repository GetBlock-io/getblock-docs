---
description: >-
  GetBlock provides fast and reliable access to Bitcoin nodes via the Bitcoin
  Core REST API. Connect to the Bitcoin network without running your own
  infrastructure.
---

# Bitcoin REST API

The Bitcoin REST API is Bitcoin Core's own HTTP interface, served directly by the node. Each endpoint is a path under `/rest/`, takes its arguments in the path rather than a query string, and ends with the response format. It is read-only: it reads blocks, transactions, outpoints, the mempool, and consensus state.

{% hint style="info" %}
This is the **node's** REST interface, not a block explorer API and not Blockbook. It has no address index, so it cannot answer "what is the balance of this address" or "what are the unspent outputs of this address". Those queries belong on the [Blockbook add-on](../bitcoin-blockbook-rest-api/), which maintains that index separately.
{% endhint %}

### Base URL

```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/
```

{% hint style="info" %}
The same access token also serves [JSON-RPC](../bitcoin-btc-json-rpc-api/) at the root of the endpoint. The two are different shapes over one endpoint: `GET` a `/rest/` path, or `POST` a JSON-RPC body to `/`. Blockbook is the exception — it is a separate add-on with its own token and URL.
{% endhint %}

### Response Formats

Every path ends with a format suffix:

| Suffix  | Content type               | Use                                        |
| ------- | -------------------------- | ------------------------------------------ |
| `.json` | `application/json`         | Decoded output, as documented on each page |
| `.hex`  | `text/plain`               | The object serialized and hex-encoded      |
| `.bin`  | `application/octet-stream` | The object serialized as raw bytes         |

`.hex` and `.bin` apply to blocks, headers, and transactions. `.bin` is the most compact way to pull a block for local parsing.

### Endpoints

| Endpoint                                | Description                                             |
| --------------------------------------- | ------------------------------------------------------- |
| `/rest/chaininfo.json`                  | Chain height, best block, difficulty, and sync progress |
| `/rest/blockhashbyheight/{height}.json` | The block hash at a given height                        |
| `/rest/block/notxdetails/{hash}.json`   | Block header fields plus the list of transaction ids    |
| `/rest/block/{hash}.json`               | Block with every transaction decoded in full            |
| `/rest/headers/{count}/{hash}.json`     | A run of block headers walking forward from a block     |
| `/rest/tx/{txid}.json`                  | A transaction, decoded                                  |
| `/rest/getutxos/{txid}-{n}.json`        | Whether given outpoints are unspent                     |
| `/rest/mempool/info.json`               | Mempool size, memory usage, and fee floors              |
| `/rest/mempool/contents.json`           | Every mempool transaction with fee and ancestry detail  |
| `/rest/deploymentinfo.json`             | Activation state of each consensus soft fork            |

### Quick check

This request returns the node's chain state and confirms the endpoint is configured correctly:

{% code overflow="wrap" %}
```bash
curl --location 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/chaininfo.json'
```
{% endcode %}

### What this interface cannot do

| Need                                     | Why REST cannot                 | Use instead                                                                                                                                                 |
| ---------------------------------------- | ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Address balance, history, or UTXOs       | The node keeps no address index | Blockbook [api/v2/address](../bitcoin-blockbook-rest-api/api-v2-address-bitcoin.md) and [api/v2/utxo](../bitcoin-blockbook-rest-api/api-v2-utxo-bitcoin.md) |
| Broadcasting a transaction               | The REST interface is read-only | JSON-RPC `sendrawtransaction`, or Blockbook [api/v2/sendtx](../bitcoin-blockbook-rest-api/api-v2-sendtx-bitcoin.md)                                         |
| A transaction's fee or confirmations     | `rest/tx` returns neither       | JSON-RPC [getrawtransaction](../bitcoin-btc-json-rpc-api/getrawtransaction-bitcoin.md) with verbosity `2`                                                   |
| Fee estimates                            | Not exposed over REST           | JSON-RPC [estimatesmartfee](../bitcoin-btc-json-rpc-api/estimatesmartfee-bitcoin.md)                                                                        |
| Subscriptions to new blocks or addresses | REST is request/response only   | Blockbook [WebSocket](../bitcoin-blockbook-websocket-api/)                                                                                                  |

### Notes

* **Amounts are in BTC as decimals**, not satoshis. Parsing them as floating-point numbers loses precision. Blockbook reports integer satoshi strings instead.
* **Arguments go in the path**, not the query string, and the format suffix is required.
* **Responses can be very large.** A full block runs to megabytes, and `mempool/contents` larger still. Prefer `block/notxdetails` and `mempool/info` where they will do.

### Support

* Support: [support@getblock.io](mailto:support@getblock.io)
