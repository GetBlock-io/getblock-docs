---
description: >-
  Example code for the signmessagewithprivkey  {disallowed} json-rpc method.
  Сomplete guide on how to use signmessagewithprivkey  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# signmessagewithprivkey {disallowed} - Dash

#### Parameters

`Private Key` - string (base58)

The private key to sign the message with encoded in base58check using wallet import format (WIF).

`Message` - string

The message to sign.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "signmessagewithprivkey",
"params": ["privatekeywithbas58", "my message"],
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
