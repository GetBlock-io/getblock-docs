---
description: >-
  Example code for the signmessage  {disallowed} json-rpc method. Сomplete guide
  on how to use signmessage  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# signmessage {disallowed} - Dash

#### Parameters

`Address` - string (base58)

A P2PKH address whose private key belongs to this wallet.

`Message` - string

The message to sign

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "signmessage",
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
