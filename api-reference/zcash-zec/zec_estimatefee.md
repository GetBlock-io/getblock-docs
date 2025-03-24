---
description: >-
  Example code for the estimatefee json-rpc method. Сomplete guide on how to use
  estimatefee json-rpc in GetBlock.io Web3 documentation.
---

# estimatefee - Zcash

#### Parameters

`nblocks` - numeric

None

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "estimatefee",
"params": [5],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": 0.0001005
}
```
