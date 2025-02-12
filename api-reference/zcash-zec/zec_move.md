---
description: >-
  Example code for the move  {disallowed} json-rpc method. Сomplete guide on how
  to use move  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# move {disallowed} - Zcash

#### Parameters

`fromaccount` - string

MUST be set to the empty string "" to represent the default account.

`toaccount` - string

MUST be set to the empty string "" to represent the default account.

`amount` - numeric

Quantity of ZEC to move between accounts.

`minconf` - None

Optional, default=1

Only use funds with at least this many confirmations.

`comment` - None

Optional

An optional comment, stored in the wallet only.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
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
