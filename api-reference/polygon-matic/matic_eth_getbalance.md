---
description: >-
  Example code for the eth_getBalance json-rpc method. Сomplete guide on how to
  use eth_getBalance json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBalance - Polygon

#### Parameters

`DATA` - string

address to check for balance.

`QUANTITY|TAG` - integer or string

block number or "latest", "earliest" or "pending"

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getBalance",
"params": ["0xe7e2cb8c81c10ff191a73fe266788c9ce62ec754", "latest"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x14be94a41983d79411b7"
}
```
