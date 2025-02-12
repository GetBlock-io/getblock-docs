---
description: >-
  Example code for the debug  {disallowed} json-rpc method. Сomplete guide on
  how to use debug  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# debug {disallowed} - Dash

#### Parameters

`Debug category` - string

The debug category to activate. Use a + to specify multiple categories.

Categories will be one of the following:

\- 0 - Disables all categories

\- 1 or all - Enables all categories

\- addrman

\- bench

\- cmpctblock

\- coindb

\- db

\- estimatefee

\- http

\- leveldb

\- libevent

\- mempool

\- mempoolrej

\- net

\- proxy

\- prune

\- qt

\- rand

\- reindex

\- rpc

\- selections

\- tor

\- zmq

\- dash (all subcategories)

The dash sub-categories can be enabled individually:

\- chainlocks

\- gobject

\- instantsend

\- keepasa

\- llmq

\- llmq-dkg

\- llmq-sigs

\- mnpayments

\- mnsync

\- coinjoin

\- spork

Note: No error will be thrown even if the specified category does not match any of the above.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "debug",
"params": [null],
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
