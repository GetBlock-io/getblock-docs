---
description: >-
  Example code for the listaccounts  {disallowed} json-rpc method. Сomplete
  guide on how to use listaccounts  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# listaccounts {disallowed} - Dogecoin

#### Parameters

`minconf` - integer

Minimum number of confirmations before payments are included.

`as_dict` - boolean

Optional, default=false

Returns a dictionary of account names, with their balance as values.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "listaccounts",
"params": [1, false],
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
