---
description: >-
  Example code for the createwallet  {disallowed} json-rpc method. Сomplete
  guide on how to use createwallet  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# createwallet {disallowed} - Bitcoin Cash

#### Parameters

`wallet_name` - string, required

The name for the new wallet. If this is a path, the wallet will be created at the path location.

`disable_private_keys` - boolean, optional, default=false

Disable the possibility of private keys (only watchonlys are possible in this mode).

`blank` - boolean, optional, default=false

Create a blank wallet. A blank wallet has no keys or HD seed. One can be set using sethdseed.

`passphrase` - string

Encrypt the wallet with this passphrase.

`avoid_reuse` - boolean, optional, default=false

Keep track of coin reuse, and treat dirty and clean coins differently with privacy considerations in mind.

`descriptors` - boolean, optional, default=false

Create a native descriptor wallet. The wallet will use descriptors internally to handle address creation

`load_on_startup` - boolean, optional, default=null

Save wallet name to persistent settings and load on startup. True to add wallet to startup list, false to remove, null to leave unchanged.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "createwallet",
"params": [null, null, null, null, null, null, null],
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
