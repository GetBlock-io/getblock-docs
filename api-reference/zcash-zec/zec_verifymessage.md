---
description: >-
  Example code for the verifymessage json-rpc method. Сomplete guide on how to
  use verifymessage json-rpc in GetBlock.io Web3 documentation.
---

# verifymessage - Zcash

#### Parameters

`zcashaddress` - string

The Zcash address to use for the signature.

`signature` - string

The signature provided by the signer in base 64 encoding (see signmessage).

`message` - string

The message that was signed.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "verifymessage",
"params": ["t1L2rjgGrvEqfrA5zqUca4GGxAeQg47CTpG", "signature", "my message"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": true
}
```
