---
description: >-
  Example code for the upgradetohd  {disallowed} json-rpc method. Сomplete guide
  on how to use upgradetohd  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# upgradetohd {disallowed} - Dash

#### Parameters

`Mnemonic` - string

Optional.

Mnemonic as defined in BIP39 to use for the new HD wallet. Use an empty string "" to generate a new random mnemonic.

`Mnemonic Passphrase` - string

Optional.

Optional mnemonic passphrase as defined in BIP39.

`Wallet Passphrase` - string

Optional.

If your wallet is encrypted you must have your wallet passphrase here. If your wallet is not encrypted, specifying wallet passphrase will trigger wallet encryption.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "upgradetohd",
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
