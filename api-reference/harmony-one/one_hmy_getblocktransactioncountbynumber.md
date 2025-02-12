---
description: >-
  Example code for the hmy_getBlockTransactionCountByNumber json-rpc method.
  Сomplete guide on how to use hmy_getBlockTransactionCountByNumber json-rpc in
  GetBlock.io Web3 documentation.
---

# hmy\_getBlockTransactionCountByNumber - Harmony

#### Parameters

`QUANTITY|TAG` - hex string

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

#### Request

```java
curl --location --request POST 'https://one.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "hmy_getBlockTransactionCountByNumber",
"params": ["0x1AC46E3"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0xc"
}
```
