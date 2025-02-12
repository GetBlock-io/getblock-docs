---
description: >-
  Example code for the sendtoaddress  {disallowed} json-rpc method. Сomplete
  guide on how to use sendtoaddress  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# sendtoaddress {disallowed} - Zcash

#### Parameters

`zcashaddress` - string

The Zcash address to send to.

`amount` - numeric

The amount in ZEC (transaction fee is added on top).

`comment` - string

Optional.

A comment used to store what the transaction is for. This is not part of the transaction, just kept in your wallet.

`comment-to` - string

Optional.

An optional comment to store the name of the person or organization to which you're sending the transaction. This is not part of the transaction, it is just kept in your wallet.

`subtractfeefromamount` - boolean

Optional, default=false

The fee will be deducted from the amount being sent.

The recipient will receive less Zcash than you enter in the amount field.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "sendtoaddress",
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
