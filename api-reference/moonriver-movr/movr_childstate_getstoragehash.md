---
description: >-
  Example code for the childstate_getStorageHash  {disallowed} json-rpc method.
  Сomplete guide on how to use childstate_getStorageHash  {disallowed} json-rpc
  in GetBlock.io Web3 documentation.
---

# childstate\_getStorageHash {disallowed} - Moonriver

#### Parameters

`childKey` - PrefixedStorageKey

None

`key` - StorageKey

None

`at` - Hash

None

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "childstate_getStorageHash",
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
