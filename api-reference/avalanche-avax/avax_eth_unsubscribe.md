---
description: >-
  Example code for the eth_unsubscribe json-rpc method. Сomplete guide on how to
  use eth_unsubscribe json-rpc in GetBlock.io Web3 documentation.
---

# eth\_unsubscribe - Avalanche

#### Parameters

`data` - hex string

A subscription ID previously generated with eth\_subscribe method.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc' \ 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_unsubscribe",
"params": ["0xe5af64ddfd365b4632988c5935cfedb7"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": {
        "code": -32000,
        "message": "subscription not found"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
