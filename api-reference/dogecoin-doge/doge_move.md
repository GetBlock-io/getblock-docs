---
description: >-
  Example code for the move  {disallowed} json-rpc method. Сomplete guide on how
  to use move  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# move {disallowed} - Dogecoin

#### Parameters

`fromaccount` - string

Source account name.

`toaccount` - string

Destination account name.

`amount` - integer

Amount to transfer.

`minconf` - integer

Minimum number of confirmations required for transferred balance.

`Comment` - string

Comment to add to transaction log.

#### Request

```java
curl --location --request POST 'https://doge.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "move",
"params": [null, null, null, null, null],
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
