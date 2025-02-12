---
description: >-
  Example code for the prioritisetransaction  {disallowed} json-rpc method.
  Сomplete guide on how to use prioritisetransaction  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# prioritisetransaction {disallowed} - Bitcoin Cash

#### Parameters

`txid` - string, required

The transaction id.

`dummy` - numeric, optional

DEPRECATED. For forward compatibility use named arguments and omit this parameter.

`fee_delta` - numeric, required

The fee value (in satoshis) to add (or subtract, if negative).

#### Request

```java
curl --location --request POST 'https://bch.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "prioritisetransaction",
"params": [null, null, null],
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
