---
description: >-
  Example code for the joinpsbts json-rpc method. Сomplete guide on how to use
  joinpsbts json-rpc in GetBlock.io Web3 documentation.
---

# joinpsbts - Bitcoin Gold

#### Parameters

`txs` - json array, required

The base64 strings of partially signed transactions

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "joinpsbts",
"params": [["mybase64_1", "mybase64_2", "mybase64_3"]],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": null
}
```
