---
description: >-
  Example code for the eth_getCode json-rpc method. Сomplete guide on how to use
  eth_getCode json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getCode - Avalanche

#### Parameters

`DATA` - string

address.

`QUANTITY|TAG` - string

block number or "latest", "earliest" or "pending"

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/mainnet/ext/bc/C/rpc' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "eth_getCode",
  "params": [
    "0x0f8d94cea1eba9c582bbe800545ca91dfc39da18",
    "latest"
  ],
  "id": "getblock.io"
}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x"
}
```
