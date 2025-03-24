---
description: >-
  Example code for the getbestblockhash json-rpc method. Сomplete guide on how
  to use getbestblockhash json-rpc in GetBlock.io Web3 documentation.
---

# getbestblockhash - Bitcoin Gold

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getbestblockhash",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": "000000007cc1fc836d0992202730ca71f77a8cf2a35974ca300dadbf8bc1e090"
}
```
