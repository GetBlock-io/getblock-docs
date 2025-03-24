---
description: >-
  Example code for the eth_getStorageAt json-rpc method. Сomplete guide on how
  to use eth_getStorageAt json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getStorageAt - KuCoin Community Chain

#### Parameters

`DATA` - string

address to check for balance.

`QUANTITY` - integer

Position in the storage.

`QUANTITY|TAG` - string

block number or "latest", "earliest" or "pending"

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getStorageAt",
"params": ["0xbec4e80531dad6a9989828d174cc2878b1e3123d", "0x0", "earliest"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x00000000000000000000000000000000000000000000000000000000000004d2"
}
```
