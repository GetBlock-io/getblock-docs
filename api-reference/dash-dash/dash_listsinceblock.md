---
description: >-
  Example code for the listsinceblock  {disallowed} json-rpc method. Сomplete
  guide on how to use listsinceblock  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# listsinceblock {disallowed} - Dash

#### Parameters

`Header Hash` - string (hex)

Optional.

The hash of a block header encoded as hex in RPC byte order. All transactions affecting the wallet which are not in that block or any earlier block will be returned, including unconfirmed transactions. Default is the hash of the genesis block, so all transactions affecting the wallet are returned by default.

`Target Confirmations` - number (int)

Optional.

Sets the lastblock field of the results to the header hash of a block with this many confirmations. This does not affect which transactions are returned.

Default is 1, so the hash of the most recent block on the local best block chain is returned.

`Include Watch-Only` - bool

Optional.

If set to true, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.

If set to false (the default), treat watch-only addresses as if they didn't belong to this wallet.

`include_removed` - bool

Optional. Default=true

Show transactions that were removed due to a reorg in the "removed" array (not guaranteed to work on pruned nodes).

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
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
