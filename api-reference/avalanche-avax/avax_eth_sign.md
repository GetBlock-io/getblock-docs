---
description: >-
  Example code for the eth_sign  {disallowed} json-rpc method. Сomplete guide on
  how to use eth_sign  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# eth\_sign {disallowed} - Avalanche

#### Parameters

`DATA` - string

address.

`DATA` - string

message to sign.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/ext/bc/C/rpc' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_sign",
"params": ["0x0f8d94cea1eba9c582bbe800545ca91dfc39da18", "0xdeadbeaf"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```
