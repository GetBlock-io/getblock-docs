---
description: >-
  Example code for the getreceivedbyaccount  {disallowed} json-rpc method.
  Сomplete guide on how to use getreceivedbyaccount  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# getreceivedbyaccount {disallowed} - Dogecoin

#### Parameters

`account` - string

Account to query for total amount.

`minconf` - integer

Number of confirmations to require, defaults to 1.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getreceivedbyaccount",
"params": [null, 1],
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
