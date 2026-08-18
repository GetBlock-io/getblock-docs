---
description: >-
  Example code for the EXPERIMENTAL_protocol_config  {disallowed} json-rpc
  method. Сomplete guide on how to use EXPERIMENTAL_protocol_config 
  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# EXPERIMENTAL\_protocol\_config {disallowed} - NEAR Protocol

#### Parameters

`finality` - string

You should pick either that or block\_id.

Can be either "optimistic" or "final"

`block_id` - int or string

You should pick either that or finality.

The block\_id param can take either the block number OR the block hash as an argument.

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "EXPERIMENTAL_protocol_config",
"params": ["final", null],
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
