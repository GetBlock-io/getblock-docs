---
description: >-
  Example code for the backupwallet  {disallowed} json-rpc method. Сomplete
  guide on how to use backupwallet  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# backupwallet {disallowed} - Dash

#### Parameters

`Destination` - string

A filename or directory name. If a filename, it will be created or overwritten. If a directory name, the file wallet.dat will be created or overwritten within that directory.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "backupwallet",
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
