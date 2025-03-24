---
description: >-
  Example code for the listreceivedbyaddress  {disallowed} json-rpc method.
  Сomplete guide on how to use listreceivedbyaddress  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# listreceivedbyaddress {disallowed} - Zcash

#### Parameters

`minconf` - numeric

Optional, default=1

The minimum number of confirmations before payments are included.

`includeempty` - boolean

Optional, default=false

Whether to include accounts that haven't received any payments.

`includeWatchonly` - boolean

Optional, default=false

Whether to include watchonly addresses.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "listreceivedbyaddress",
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
