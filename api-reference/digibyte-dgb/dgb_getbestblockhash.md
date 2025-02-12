---
description: >-
  Example code for the getbestblockhash json-rpc method. Сomplete guide on how
  to use getbestblockhash json-rpc in GetBlock.io Web3 documentation.
---

# getbestblockhash - DigiByte

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://dgb.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
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
    "result": "b034141fc8e8024752b39513c9ed6db14690bf3a7fe9911a89c83578a73f0457"
}
```
