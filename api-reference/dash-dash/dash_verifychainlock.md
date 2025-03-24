---
description: >-
  Example code for the verifychainlock  {disallowed} json-rpc method. Сomplete
  guide on how to use verifychainlock  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# verifychainlock {disallowed} - Dash

#### Parameters

`blockhash` - string (hex)

The block hash of the ChainLock.

`signature` - string (hex)

The ChainLock signature to verify.

`blockHeight` - number

Optional.

The height of the ChainLock. There will be an internal lookup of blockHash if this is not provided.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "verifychainlock",
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
