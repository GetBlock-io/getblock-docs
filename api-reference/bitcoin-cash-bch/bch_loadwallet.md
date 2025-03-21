---
description: >-
  Example code for the loadwallet  {disallowed} json-rpc method. Сomplete guide
  on how to use loadwallet  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# loadwallet {disallowed} - Bitcoin Cash

#### Parameters

`filename` - string, required

The wallet directory or .dat file.

`load_on_startup` - boolean, optional, default=null

Save wallet name to persistent settings and load on startup. True to add wallet to startup list, false to remove, null to leave unchanged.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "loadwallet",
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
