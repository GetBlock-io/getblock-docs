---
description: >-
  Example code for the eth_getCode json-rpc method. Сomplete guide on how to use
  eth_getCode json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getCode - Polygon

#### Parameters

`DATA` - string

address.

`QUANTITY|TAG` - integer or string

block number or "latest", "earliest" or "pending"

#### Request

```java
curl --location --request POST 'https://matic.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getCode",
"params": ["0xe7e2cb8c81c10ff191a73fe266788c9ce62ec754", "latest"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x"
}
```
