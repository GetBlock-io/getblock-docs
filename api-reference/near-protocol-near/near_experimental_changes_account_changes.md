---
description: >-
  Example code for the EXPERIMENTAL_changes(account_changes)  {disallowed}
  json-rpc method. Сomplete guide on how to use
  EXPERIMENTAL_changes(account_changes)  {disallowed} json-rpc in GetBlock.io
  Web3
---

# EXPERIMENTAL\_changes(account\_changes) {disallowed} - NEAR Protocol

#### Parameters

`changes_type` - string

type of changes

`accounts` - array of string

an array of account ids

`finality` - string

You should pick either that or block\_id.

Can be either "optimistic" or "final"

`block_id` - int or string

You should pick either that or finality.

The block\_id param can take either the block number OR the block hash as an argument.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "EXPERIMENTAL_changes",
"params": {"changes_type": "account_changes", "finality": "final"},
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
