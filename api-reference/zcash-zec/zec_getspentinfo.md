---
description: >-
  Example code for the getspentinfo  {disallowed} json-rpc method. Сomplete
  guide on how to use getspentinfo  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# getspentinfo {disallowed} - Zcash

#### Parameters

`txid` - string

The hex string of the txid.

`index` - number

The vout (output) index.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getspentinfo",
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
