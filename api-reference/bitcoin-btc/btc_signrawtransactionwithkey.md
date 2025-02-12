---
description: >-
  Example code for the signrawtransactionwithkey  {disallowed} json-rpc method.
  Сomplete guide on how to use signrawtransactionwithkey  {disallowed} json-rpc
  in GetBlock.io Web3 documentation.
---

# signrawtransactionwithkey {disallowed} - Bitcoin

#### Parameters

`hexstring` - string, required

The transaction hex string

`privkeys` - json array, required

The base58-encoded private keys for signing

`prevtxs` - json array, optional

The previous dependent transaction outputs

`sighashtype` - string, optional, default=ALL

The signature hash type. Must be one of:

ALL

NONE

SINGLE

ALL|ANYONECANPAY

NONE|ANYONECANPAY

SINGLE|ANYONECANPAY

#### Request

```java
curl --location --request POST 'https://btc.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "signrawtransactionwithkey",
"params": ["hexstring", "privkeys", null, null],
"id": "getblock.io"}'
```
