---
description: >-
  Example code for the sendfrom  {disallowed} json-rpc method. Сomplete guide on
  how to use sendfrom  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# sendfrom {disallowed} - Zcash

#### Parameters

`fromaccount` - string

MUST be set to the empty string "" to represent the default account. Passing any other string will result in an error.

`tozcashaddress` - string

The Zcash address to send funds to.

`amount` - numeric

The amount in ZEC (transaction fee is added on top).

`minconf` - numeric

Optional, default=1

Only use funds with at least this many confirmations.

`comment` - string

Optional.

A comment used to store what the transaction is for. This is not part of the transaction, just kept in your wallet.

`comment-to` - string

Optional.

An optional comment to store the name of the person or organization to which you're sending the transaction. This is not part of the transaction, it is just kept in your wallet.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "sendfrom",
"params": [null, null, null, null, null, null],
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
