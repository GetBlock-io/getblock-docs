---
description: >-
  Example code for the childstate_getStorageEntries  {disallowed} json-rpc
  method. Сomplete guide on how to use childstate_getStorageEntries 
  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# childstate\_getStorageEntries {disallowed} - Moonriver

#### Parameters

`childKey` - PrefixedStorageKey

None

`keys` - list of StorageKey

None

`at` - hash

optional

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "childstate_getStorageEntries",
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
