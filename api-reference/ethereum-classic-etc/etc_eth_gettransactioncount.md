---
description: >-
  Example code for the eth_getTransactionCount json-rpc method. Сomplete guide
  on how to use eth_getTransactionCount json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionCount - Ethereum Classic

#### Parameters

`data` - string

20-byte account address.

`quantity|tag` - string

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionCount",
"params": ["0x7eb4c9d6b763324eea4852f5d40985bbf0f29832", "latest"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x22c"
}
```
