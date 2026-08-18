---
description: >-
  Example code for the walletpassphrasechange  {disallowed} json-rpc method.
  Сomplete guide on how to use walletpassphrasechange  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# walletpassphrasechange {disallowed} - Dash

#### Parameters

`Current Passphrase` - string

The current wallet passphrase.

`New Passphrase` - string

The new passphrase for the wallet.

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "None",
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
