---
description: >-
  Example code for the hmy_getBalance json-rpc method. Сomplete guide on how to
  use hmy_getBalance json-rpc in GetBlock.io Web3 documentation.
---

# hmy\_getBalance - Harmony

#### Parameters

`DATA` - hex string

20-byte account address from which to retrieve the balance.

`QUANTITY|TAG` - hex string

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "hmy_getBalance",
"params": ["0x227f6757289a86c13eee2e91c2e6eb03f2ed11a6", "latest"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x0"
}
```
