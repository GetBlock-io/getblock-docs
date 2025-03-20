---
description: >-
  Example code for the eth_submitWork  {disallowed} json-rpc method. Сomplete
  guide on how to use eth_submitWork  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_submitWork {disallowed} - Avalanche

#### Parameters

`data` - string

The nonce found (64 bits)

`data` - string

The header’s pow-hash (256 bits)

`data` - string

The mix digest (256 bits)

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/ext/bc/C/rpc' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_submitWork",
"params": [null, null, null],
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
