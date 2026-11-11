---
description: >-
  Example code for the signmessagewithprivkey  {disallowed} json-rpc method.
  Сomplete guide on how to use signmessagewithprivkey  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# signmessagewithprivkey {disallowed} - Bitcoin SV

#### Parameters

`privkey` - string, required

The private key to sign the message with.

`message` - string, required

The message to create a signature of.

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "signmessagewithprivkey",
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
