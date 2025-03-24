---
description: >-
  Example code for the eth_getLogs json-rpc method. Сomplete guide on how to use
  eth_getLogs json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getLogs - Fantom

#### Parameters

`Object` - hex string

Filter options object.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getLogs",
"params": [{"fromBlock": "0x3BE8F6D", "toBlock": "0x3BE8F6D", "address": "0xbee08753d42db191f438979d3cb6a08d28d36555", "topics": []}],
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
