---
description: >-
  Example code for the eth_getStorageAt json-rpc method. Сomplete guide on how
  to use eth_getStorageAt json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getStorageAt - Avalanche

#### Parameters

`DATA` - string

address to check for balance.

`QUANTITY` - integer

Position in the storage.

`QUANTITY|TAG` - string

block number or "latest", "earliest" or "pending"

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc' \
--header 'Content-Type: application/json' 
--data-raw '{
  "jsonrpc": "2.0",
  "method": "eth_getStorageAt",
  "params": ["0x0f8d94cea1eba9c582bbe800545ca91dfc39da18", "0x0", "latest"],
  "id": "getblock.io"
}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x0000000000000000000000000000000000000000000000000000000000000000"
}
```
