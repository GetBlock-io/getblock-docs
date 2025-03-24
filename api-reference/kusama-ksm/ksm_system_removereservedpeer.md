---
description: >-
  Example code for the system_removeReservedPeer  {disallowed} json-rpc method.
  Сomplete guide on how to use system_removeReservedPeer  {disallowed} json-rpc
  in GetBlock.io Web3 documentation.
---

# system\_removeReservedPeer {disallowed} - Kusama

#### Parameters

`peerId` - Text

None

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "system_removeReservedPeer",
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
