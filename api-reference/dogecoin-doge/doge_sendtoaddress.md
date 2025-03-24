---
description: >-
  Example code for the sendtoaddress  {disallowed} json-rpc method. Сomplete
  guide on how to use sendtoaddress  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# sendtoaddress {disallowed} - Dogecoin

#### Parameters

`dogecoinaddress` - string

Dogecoin address to send to.

`amount` - float

Amount to send (float, rounded to the nearest 0.00000001).

`minconf` - integer

Minimum number of confirmations required for transferred balance.

`Comment` - string

Optional.

Comment for transaction.

`Comment_to` - string

Optional.

Comment for to-address.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "sendtoaddress",
"params": [null, null, 1, null, null],
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
