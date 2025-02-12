---
description: >-
  Example code for the keepass  {disallowed} json-rpc method. Сomplete guide on
  how to use keepass  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# keepass {disallowed} - Dash

#### Parameters

`mode` - string

The command mode to use:

\- genkey - Generates a base64 encoded 256 bit AES key that can be used for the communication with KeePassHttp. This is only necessary for manual configuration.

\- init - Sets up the association between Dash Core and KeePass by generating an AES key and sending an association message to KeePassHttp. This will trigger KeePass to ask for an Id for the association. Returns the association and the base64 encoded string for the AES key.

\- setpassphrase - Updates the passphrase in KeePassHttp to a new value. This should match the passphrase you intend to use for the wallet. Please note that the standard RPC commands walletpassphrasechange and the wallet encryption from the QT GUI already send the updates to KeePassHttp, so this is only necessary for manual manipulation of the password.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "keepass",
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
