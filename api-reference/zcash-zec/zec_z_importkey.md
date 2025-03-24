---
description: >-
  Example code for the z_importkey  {disallowed} json-rpc method. Сomplete guide
  on how to use z_importkey  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# z\_importkey {disallowed} - Zcash

#### Parameters

`zkey` - string

The zkey (see z\_exportkey).

`rescan` - boolean

Optional, default="whenkeyisnew"

Rescan the wallet for transactions - can be "yes", "no" or "whenkeyisnew"

`startHeight` - numeric

Optional, default=0

Block height to start rescan from.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "z_importkey",
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
