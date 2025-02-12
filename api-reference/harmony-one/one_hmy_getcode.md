---
description: >-
  Example code for the hmy_getCode json-rpc method. Сomplete guide on how to use
  hmy_getCode json-rpc in GetBlock.io Web3 documentation.
---

# hmy\_getCode - Harmony

#### Parameters

`DATA` - hex string

20-byte contract address.

`QUANTITY|TAG` - hex string

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

#### Request

```java
curl --location --request POST 'https://one.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "hmy_getCode",
"params": ["0x3df33217f0f82c99ff3ff448512f22cef39cc208", "latest"],
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
