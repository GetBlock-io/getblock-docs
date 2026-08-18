---
description: >-
  Example code for the miner_changeTargetGasLimit  {disallowed} json-rpc method.
  Сomplete guide on how to use miner_changeTargetGasLimit  {disallowed} json-rpc
  in GetBlock.io Web3 documentation.
---

# miner\_changeTargetGasLimit {disallowed} - Ethereum Classic

#### Parameters

`quantity` - integer

The target gas price in Wei.

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "miner_changeTargetGasLimit",
"params": [null],
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
