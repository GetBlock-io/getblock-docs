---
description: >-
  Example code for the sendtoaddress  {disallowed} json-rpc method. Сomplete
  guide on how to use sendtoaddress  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# sendtoaddress {disallowed} - Bitcoin SV

#### Parameters

`address` - string, required

The bitcoin address to send to.

`amount` - numeric or string, required

The amount in BTC to send. eg 0.1

`comment` - string, optional

A comment used to store what the transaction is for.

`comment_to` - string, optional

A comment to store the name of the person or organization

`subtractfeefromamount` - boolean, optional, default=false

The fee will be deducted from the amount being sent.

`replaceable` - boolean, optional, default=wallet default

Allow this transaction to be replaced by a transaction with higher fees via BIP 125

`conf_target` - numeric, optional, default=wallet -txconfirmtarget

Confirmation target in blocks

`estimate_mode` - string, optional, default=unset

The fee estimate mode, must be one of (case insensitive) “unset” “economical” “conservative”

`avoid_reuse` - boolean, optional, default=true

(only available if avoid\_reuse wallet flag is set) Avoid spending from dirty addresses; addresses are considered

dirty if they have previously been used in a transaction.

#### Request

```java
curl --location --request POST 'https://bsv.getblock.io/mainnet/' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "sendtoaddress",
"params": [null, null, null, null, null, null, null, null, null],
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
