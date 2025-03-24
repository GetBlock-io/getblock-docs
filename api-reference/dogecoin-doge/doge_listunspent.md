---
description: >-
  Example code for the listunspent  {disallowed} json-rpc method. Сomplete guide
  on how to use listunspent  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# listunspent {disallowed} - Dogecoin

#### Parameters

`minconf` - integer

Minimum number of confirmations required to be listed.

`maxconf` - integer

Maximal number of confirmations allowed to be listed.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "listunspent",
"params": [1, 999999],
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
