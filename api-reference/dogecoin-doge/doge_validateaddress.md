---
description: >-
  Example code for the validateaddress json-rpc method. Сomplete guide on how to
  use validateaddress json-rpc in GetBlock.io Web3 documentation.
---

# validateaddress - Dogecoin

#### Parameters

`address` - string

Address to validate.

#### Request

```java
curl --location --request POST 'https://doge.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "validateaddress",
"params": ["DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "address": "DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH",
        "ismine": false,
        "isscript": false,
        "isvalid": true,
        "iswatchonly": false,
        "scriptPubKey": "76a914a5f4d12ce3685781b227c1f39548ddef429e978388ac"
    }
}
```
