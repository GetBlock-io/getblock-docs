---
description: >-
  Example code for the rescanblockchain  {disallowed} json-rpc method. Сomplete
  guide on how to use rescanblockchain  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# rescanblockchain {disallowed} - Dash

#### Parameters

`Start Height` - integer

Optional.

The block height where the rescan should start.

`Stop Height` - integer

Optional.

The last block height that should be scanned.

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "rescanblockchain",
"params": [null, null],
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
