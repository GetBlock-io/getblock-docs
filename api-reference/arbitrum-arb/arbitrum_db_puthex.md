---
description: >-
  Example code for the db_putHex  {disallowed} json-rpc method. Сomplete guide
  on how to use db_putHex  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# db\_putHex {disallowed} - Arbitrum

#### Parameters

`database` - string

database name

`key` - string

key name

`data` - data

data to store.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "db_putHex", "params": [null, null, null], "id": "getblock.io"}'
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
