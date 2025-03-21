---
description: >-
  Example code for the walletpassphrase  {disallowed} json-rpc method. Сomplete
  guide on how to use walletpassphrase  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# walletpassphrase {disallowed} - Bitcoin

#### Parameters

`passphrase` - string, required

The wallet passphrase

`timeout` - numeric, required

The time to keep the decryption key in seconds; capped at 100000000 (\~3 years).

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "walletpassphrase",
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
