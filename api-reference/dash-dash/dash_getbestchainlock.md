---
description: >-
  Example code for the getbestchainlock json-rpc method. Сomplete guide on how
  to use getbestchainlock json-rpc in GetBlock.io Web3 documentation.
---

# getbestchainlock - Dash

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getbestchainlock",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "blockhash": "0000000000000019cd3a50eedc418372f7aa9060d6ded56a769a51a93dfe0e4a",
        "height": 1535914,
        "known_block": true,
        "signature": "0d84323152cf83b5f06ed1a9394fada874e07e31d36ecab1c4aaaf0c16f7b5b794da20d0912e1de7f47f36978835b2b61701686935ddd73da0ee21164d6d10d7c1db2c0ddbf29f9276c946d178b853f410bf9e1492e076c3eb21fa47bcd4485d"
    }
}
```
