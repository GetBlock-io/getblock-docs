---
description: >-
  Example code for the verifychain  {disallowed} json-rpc method. Сomplete guide
  on how to use verifychain  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# verifychain {disallowed} - Zcash

#### Parameters

`checklevel` - numeric

Optional.

0-4, default=3

How thorough the block verification is.

`numblocks` - numeric

Optional.

default=288, 0=all

The number of blocks to check.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "verifychain",
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
