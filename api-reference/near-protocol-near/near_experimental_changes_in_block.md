---
description: >-
  Example code for the EXPERIMENTAL_changes_in_block  {disallowed} json-rpc
  method. Сomplete guide on how to use EXPERIMENTAL_changes_in_block 
  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# EXPERIMENTAL\_changes\_in\_block {disallowed} - NEAR Protocol

#### Parameters

`finality` - string

You should pick either that or block\_id.

Can be either "optimistic" or "final"

`block_id` - int or string

You should pick either that or finality.

The block\_id param can take either the block number OR the block hash as an argument.

#### Request

```java
curl --location --request POST 'https://near.getblock.io/mainnet' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "EXPERIMENTAL_changes_in_block",
"params": [{"finality": "final"}],
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
