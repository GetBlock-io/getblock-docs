---
description: >-
  Example code for the importprivkey  {disallowed} json-rpc method. Сomplete
  guide on how to use importprivkey  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# importprivkey {disallowed} - Dash

#### Parameters

`Private key` - string (base58)

The private key to import into the wallet encoded in base58check using wallet import format (WIF).

`Account` - string

Optional.

The name of an account to which transactions involving the key should be assigned. The default is the default account, an empty string.

`Rescan` - bool

Optional.

Set to true (the default) to rescan the entire local block database for transactions affecting any address or pubkey script in the wallet (including transaction affecting the newly-added address for this private key).

Set to false to not rescan the block database (rescanning can be performed at any time by restarting Dash Core with the -rescan command-line argument).

Rescanning may take several minutes. Notes: if the address for this key is already in the wallet, the block database will not be rescanned even if this parameter is set.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "importprivkey",
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
