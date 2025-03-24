---
description: >-
  Example code for the sethdseed  {disallowed} json-rpc method. Сomplete guide
  on how to use sethdseed  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# sethdseed {disallowed} - DigiByte

#### Parameters

`newkeypool` - boolean, optional, default=true

Whether to flush old unused addresses, including change addresses, from the keypool and regenerate it.

`seed` - string, optional, default=random seed

The WIF private key to use as the new HD seed.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "sethdseed",
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
