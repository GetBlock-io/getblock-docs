---
description: >-
  Example code for the hmyv2_getBalance json-rpc method. Сomplete guide on how
  to use hmyv2_getBalance json-rpc in GetBlock.io Web3 documentation.
---

# hmyv2\_getBalance - Harmony

#### Parameters

`DATA` - hex string

20-byte account address from which to retrieve the balance.

#### Request

```java
curl --location --request POST 'https://one.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "hmyv2_getBalance",
"params": ["0x227f6757289a86c13eee2e91c2e6eb03f2ed11a6"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": 0
}
```
