---
description: >-
  Example code for the sendmany  {disallowed} json-rpc method. Сomplete guide on
  how to use sendmany  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# sendmany {disallowed} - Dogecoin

#### Parameters

`fromaccount` - string

Account to send from.

`todict` - dict

Dictionary with Dogecoin addresses as keys and amounts as values.

`minconf` - integer

Minimum number of confirmations required for transferred balance.

`Comment` - string

Optional.

Comment for transaction.

#### Request

```java
curl --location --request POST 'https://doge.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "sendmany",
"params": [null, null, 1, null],
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
