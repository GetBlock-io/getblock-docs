---
description: >-
  Example code for the eth_getUncleByBlockHashAndIndex json-rpc method. Сomplete
  guide on how to use eth_getUncleByBlockHashAndIndex json-rpc in GetBlock.io
  Web3 documentation.
---

# eth\_getUncleByBlockHashAndIndex - Avalanche

#### Parameters

`DATA` - string

Hash of a block.

`QUANTITY` - string

the uncle’s index position.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{
  "jsonrpc": "2.0",
  "method": "eth_getUncleByBlockHashAndIndex",
  "params": ["0x3e56c97d34f03b1369c351fa6c9f57c8bfa987c7da40964fab981303e0ef5849", "0x0"],
  "id": "getblock.io"
}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": null
}
```
