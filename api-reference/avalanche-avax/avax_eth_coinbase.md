---
description: >-
  Example code for the eth_coinbase json-rpc method. Сomplete guide on how to
  use eth_coinbase json-rpc in GetBlock.io Web3 documentation.
---

# eth\_coinbase - Avalanche

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{
  "jsonrpc": "2.0",
  "method": "eth_coinbase",
  "params": [],
  "id": "getblock.io"
}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x0100000000000000000000000000000000000000"
}
```
