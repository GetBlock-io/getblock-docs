---
description: >-
  Example code for the query(view_access_key_list)  {disallowed} json-rpc
  method. Сomplete guide on how to use query(view_access_key_list)  {disallowed}
  json-rpc in GetBlock.io Web3 documentation.
---

# query(view\_access\_key\_list) {disallowed} - NEAR Protocol

#### Parameters

`request_type` - string

typ of request

`finality` - string

You should pick either that or block\_id.

`block_id` - int or string

You should pick either that or finality.

The block\_id param can take either the block number OR the block hash as an argument.

`account_id` - string

id of an account.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "query",
"params": ["view_access_key_list", "final", null, null],
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
