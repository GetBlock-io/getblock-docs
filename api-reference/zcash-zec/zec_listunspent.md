---
description: >-
  Example code for the listunspent  {disallowed} json-rpc method. Сomplete guide
  on how to use listunspent  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# listunspent {disallowed} - Zcash

#### Parameters

`minconf` - numeric

Optional, default=1

The minimum confirmations to filter.

`maxconf` - numeric

Optional, default=9999999

The maximum confirmations to filter.

`addresses` - array

A json array of Zcash addresses to filter.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "listunspent",
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
