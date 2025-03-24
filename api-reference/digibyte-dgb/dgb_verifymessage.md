---
description: >-
  Example code for the verifymessage json-rpc method. Сomplete guide on how to
  use verifymessage json-rpc in GetBlock.io Web3 documentation.
---

# verifymessage - DigiByte

#### Parameters

`address` - string, required

The btc address to use for the signature.

`signature` - string, required

The signature provided by the signer in base 64 encoding (see signmessage).

`message` - string, required

The message that was signed.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "verifymessage",
"params": ["1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX", "signature", "my message"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "isvalid": true
    }
}
```
