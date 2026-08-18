---
description: >-
  Example code for the author_insertKey  {disallowed} json-rpc method. Сomplete
  guide on how to use author_insertKey  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# author\_insertKey {disallowed} - Moonriver

#### Parameters

`keyType` - Text

key type

`suri` - Text

suri

`publicKey` - Bytes

public key

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "author_insertKey",
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
