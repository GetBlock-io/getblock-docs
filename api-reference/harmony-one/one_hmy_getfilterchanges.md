---
description: >-
  Example code for the hmy_getFilterChanges json-rpc method. Сomplete guide on
  how to use hmy_getFilterChanges json-rpc in GetBlock.io Web3 documentation.
---

# hmy\_getFilterChanges - Harmony

#### Parameters

`data` - hex string

Filter ID.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "hmy_getFilterChanges",
"params": ["0x7c31"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": {
        "code": -32000,
        "message": "filter not found"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
