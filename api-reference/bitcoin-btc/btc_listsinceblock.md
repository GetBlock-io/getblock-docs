---
description: >-
  Example code for the listsinceblock  {disallowed} json-rpc method. Сomplete
  guide on how to use listsinceblock  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# listsinceblock {disallowed} - Bitcoin

#### Parameters

`blockhash` - string, optional

If set, the block hash to list transactions since, otherwise list all transactions.

`target_confirmations` - numeric, optional, default=1

Return the nth block hash from the main chain. e.g. 1 would mean the best block hash.

Note this is not used as a filter, but only affects \[lastblock] in the return value

`include_watchonly` - boolean, optional, default=true for watch-only wallets, otherwise false

Include transactions to watch-only addresses (see ‘importaddress’)

`include_removed` - boolean, optional, default=true

Show transactions that were removed due to a reorg in the “removed” array (not guaranteed to work on pruned nodes)

#### Request

```java
curl --location --request POST 'https://btc.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "listsinceblock",
"params": [null, null, null, null],
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
