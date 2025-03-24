---
description: >-
  Example code for the query(view_account) json-rpc method. Сomplete guide on
  how to use query(view_account) json-rpc in GetBlock.io Web3 documentation.
---

# query(view\_account) - NEAR Protocol

#### Parameters

`request_type` - string

request type

`finality` - string

You should pick either that or block\_id.

Can be either "optimistic" or "final"

`block_id` - int or string

You should pick either that or finality.

The block\_id param can take either the block number OR the block hash as an argument.

`account_id` - string

account id

`method_name` - string

method name

`args_base64` - string

method arguments base64 encoded

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "query",
"params": {"request_type": "view_account", "finality": "final", "account_id": "staked.poolv1.near", "method_name": "get_num"},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "amount": "33945779367939674663889680026",
        "block_hash": "BxPRtyVJQxwSevvu7LMxv8BPdA1xNWPK9uU9syC9Qv7p",
        "block_height": 61075010,
        "code_hash": "J1arLz48fgXcGyCPVckFwLnewNH6j1uw79thsvwqGYTY",
        "locked": "43032006277761885602868523789140",
        "storage_paid_at": 0,
        "storage_usage": 710635
    }
}
```
