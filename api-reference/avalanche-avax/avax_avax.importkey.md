---
description: >-
  Example code for the avax.importKey  {disallowed} json-rpc method. Сomplete
  guide on how to use avax.importKey  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# avax.importKey {disallowed} - Avalanche

#### Parameters

`username` - string

user name

`password` - string

user password

`privateKey` - string

private key

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/mainnet/ext/bc/C/rpc' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "avax.importKey",
  "params": [null, null, null],
  "id": "getblock.io"
}'
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
