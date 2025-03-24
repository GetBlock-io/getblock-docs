---
description: >-
  Example code for the thetacli.LockKey  {disallowed} json-rpc method. Сomplete
  guide on how to use thetacli.LockKey  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# thetacli.LockKey {disallowed} - Theta Network

#### Parameters

`address` - string

The address of the account to be locked

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "thetacli.LockKey",
"params": {},
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
