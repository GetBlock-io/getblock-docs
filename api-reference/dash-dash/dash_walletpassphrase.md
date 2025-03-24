---
description: >-
  Example code for the walletpassphrase  {disallowed} json-rpc method. Сomplete
  guide on how to use walletpassphrase  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# walletpassphrase {disallowed} - Dash

#### Parameters

`Passphrase` - string

The passphrase that unlocks the wallet.

`Seconds` - number (int)

The number of seconds after which the decryption key will be automatically deleted from memory.

`Mixing Only` - bool

Optional.

If true, the wallet will be locked for sending functions but unlocked for mixing transactions (default = false).

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "walletpassphrase",
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
