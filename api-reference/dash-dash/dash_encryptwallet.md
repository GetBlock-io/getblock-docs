---
description: >-
  Example code for the encryptwallet  {disallowed} json-rpc method. Сomplete
  guide on how to use encryptwallet  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# encryptwallet {disallowed} - Dash

#### Parameters

`Passphrase` - string

The passphrase to use for the encrypted wallet. Must be at least one character.

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "encryptwallet",
"params": [null],
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
