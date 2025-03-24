---
description: >-
  Example code for the hmy_getTransactionCount json-rpc method. Сomplete guide
  on how to use hmy_getTransactionCount json-rpc in GetBlock.io Web3
  documentation.
---

# hmy\_getTransactionCount - Harmony

#### Parameters

`data` - hex string

20-byte account address.

`quantity|tag` - hex string

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "hmy_getTransactionCount",
"params": ["one1qku72aj98fskvge07h9j0gtfu9sdw62ucgff67", "0x286AC07"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": {
        "code": -32000,
        "message": "missing trie node d415596d6e84fb80bcce0cb183592969547169aac226fc8e58c4de28d38d9d5c (path )"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
