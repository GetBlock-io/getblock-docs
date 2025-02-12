---
description: >-
  Example code for the verifymessage json-rpc method. Сomplete guide on how to
  use verifymessage json-rpc in GetBlock.io Web3 documentation.
---

# verifymessage - Dash

#### Parameters

`Address` - string (base58)

The P2PKH address corresponding to the private key which made the signature. A P2PKH address is a hash of the public key corresponding to the private key which made the signature.

When the ECDSA signature is checked, up to four possible ECDSA public keys will be reconstructed from from the signature; each key will be hashed and compared against the P2PKH address provided to see if any of them match.

If there are no matches, signature validation will fail.

`Signature` - string (base58)

The signature created by the signer encoded as base-64 (the format output by the signmessage RPC)

`Message` - string

The message exactly as it was signed (e.g. no extra whitespace).

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "verifymessage",
"params": ["yNpezfFDfoikDuT1f4iK75AiLp2YLPsGAb", "H4XULzfHCf16In2ECk9Ta9QxQPq639zQto2JA3OLlo3JbUdrClvJ89+A1z+Z9POd6l8LJhn1jGpQYF8mX4jkQvE=", "Hello World!"],
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
