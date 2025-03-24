---
description: >-
  Example code for the offchain_localStorageGet  {disallowed} json-rpc method.
  Сomplete guide on how to use offchain_localStorageGet  {disallowed} json-rpc
  in GetBlock.io Web3 documentation.
---

# offchain\_localStorageGet {disallowed} - Moonriver

#### Parameters

`kind` - StorageKind

None

`key` - Bytes

None

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "offchain_localStorageGet",
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
