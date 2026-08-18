---
description: >-
  Example code for the signmessage  {disallowed} json-rpc method. Сomplete guide
  on how to use signmessage  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# signmessage {disallowed} - Bitcoin Gold

#### Parameters

`address` - string, required

The bitcoin address to use for the private key.

`message` - string, required

The message to create a signature of.

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
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
