---
description: >-
  Example code for the walletpassphrase  {disallowed} json-rpc method. Сomplete
  guide on how to use walletpassphrase  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# walletpassphrase {disallowed} - Dogecoin

#### Parameters

`passphrase` - string

The wallet passphrase.

`timeout` - integer

Time in seconds to keep the wallet unlocked (by keeping the passphrase in memory).

`dont_raise` - boolean

Optional, default=false

Instead of raising \~dogecoinrpc.exceptions.WalletPassphraseIncorrect return False.

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
