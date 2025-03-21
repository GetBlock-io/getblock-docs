---
description: >-
  Example code for the parkblock  {disallowed} json-rpc method. Сomplete guide
  on how to use parkblock  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# parkblock {disallowed} - Bitcoin Cash

#### Parameters

`blockhash` - string, required

the hash of the block to park

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "parkblock",
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
