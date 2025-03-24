---
description: >-
  Example code for the importpubkey  {disallowed} json-rpc method. Сomplete
  guide on how to use importpubkey  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# importpubkey {disallowed} - Dash

#### Parameters

`Public Key` - string (hex)

The public key to import.

`Label` - string

Optional.

The label the key should be assigned.

`Rescan` - bool

Optional.

Set to true (the default) to rescan the entire local block database for transactions affecting any address or pubkey script in the wallet (including transaction affecting the newly-added address for this private key).

Set to false to not rescan the block database (rescanning can be performed at any time by restarting Dash Core with the -rescan command-line argument).

Rescanning may take several minutes. Notes: if the address for this key is already in the wallet, the block database will not be rescanned even if this parameter is set.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "importpubkey",
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
