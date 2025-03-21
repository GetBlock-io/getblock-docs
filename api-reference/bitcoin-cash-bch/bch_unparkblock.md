---
description: >-
  Example code for the unparkblock  {disallowed} json-rpc method. Сomplete guide
  on how to use unparkblock  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# unparkblock {disallowed} - Bitcoin Cash

#### Parameters

`blockhash` - string, required

the hash of the block to unpark

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "unparkblock",
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
