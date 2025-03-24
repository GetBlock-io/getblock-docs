---
description: >-
  Example code for the generate  {disallowed} json-rpc method. Сomplete guide on
  how to use generate  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# generate {disallowed} - Dash

#### Parameters

`numblocks` - number (int)

The number of blocks to generate. The RPC call will not return until all blocks have been generated.

`maxtries` - number (int)

The number of iterations to try (default = 1000000).

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "generate",
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
