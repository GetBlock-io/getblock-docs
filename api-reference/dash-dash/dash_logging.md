---
description: >-
  Example code for the logging  {disallowed} json-rpc method. Сomplete guide on
  how to use logging  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# logging {disallowed} - Dash

#### Parameters

`include` - array of strings

Optional.

Enable debugging for these categories:

\- Special:

\- - 0 - disables all categories

\- - 1 or all - enables all categories

\- - dash - Enables/Disables all Dash categories

\- Standart: addrman, bench, cmpctblock, coindb, db, estimatefee, http, leveldb, libevent, mempool, mempoolrej, net, proxy, prune, qt, rand, reindex, rpc, selectcoins, tor, zmq

\- Dash: chainlocks, gobject, instantsend, keepass, llmq, llmq-dkg, llmq-sigs, mnpayments, mnsync, coinjoin, spork

`exclude` - array of strings

Optional.

Enable debugging for these categories:

\- Special:

\- - 0 - disables all categories

\- - 1 or all - enables all categories

\- - dash - Enables/Disables all Dash categories

\- Standart: addrman, bench, cmpctblock, coindb, db, estimatefee, http, leveldb, libevent, mempool, mempoolrej, net, proxy, prune, qt, rand, reindex, rpc, selectcoins, tor, zmq

\- Dash: chainlocks, gobject, instantsend, keepass, llmq, llmq-dkg, llmq-sigs, mnpayments, mnsync, coinjoin, spork

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "logging",
"params": [null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```
