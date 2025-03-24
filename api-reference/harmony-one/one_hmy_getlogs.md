---
description: >-
  Example code for the hmy_getLogs json-rpc method. Сomplete guide on how to use
  hmy_getLogs json-rpc in GetBlock.io Web3 documentation.
---

# hmy\_getLogs - Harmony

#### Parameters

`Object` - hex string

Filter options object.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "hmy_getLogs",
"params": [{"fromBlock": "0x8252EC", "toBlock": "0x8252EC", "address": "0x227f6757289a86c13eee2e91c2e6eb03f2ed11a6", "topics": []}],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": []
}
```
