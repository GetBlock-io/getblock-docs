---
description: >-
  Example code for the getbestblockhash json-rpc method. Сomplete guide on how
  to use getbestblockhash json-rpc in GetBlock.io Web3 documentation.
---

# getbestblockhash - Dash

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
    "result": "0000000000000019cd3a50eedc418372f7aa9060d6ded56a769a51a93dfe0e4a"
}
```
