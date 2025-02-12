---
description: >-
  Example code for the walletpassphrasechange  {disallowed} json-rpc method.
  Сomplete guide on how to use walletpassphrasechange  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# walletpassphrasechange {disallowed} - Bitcoin SV

#### Parameters

`oldpassphrase` - string, required

The current passphrase

`newpassphrase` - string, required

The new passphrase

#### Request

```java
curl --location --request POST 'https://bsv.getblock.io/mainnet/' / 
--header 'x-api-key: YOUR-API-KEY' / 
--header 'Content-Type: application/json' / 
--data-raw '{"jsonrpc": "2.0",
"method": "walletpassphrasechange",
"params": [null, null],
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
