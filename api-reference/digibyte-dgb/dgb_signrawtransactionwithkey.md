---
description: >-
  Example code for the signrawtransactionwithkey json-rpc method. Сomplete guide
  on how to use signrawtransactionwithkey json-rpc in GetBlock.io Web3
  documentation.
---

# signrawtransactionwithkey - DigiByte

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
curl --location --request POST 'https://dgb.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "signrawtransactionwithkey",
"params": ["010000000001010000000000000000000000000000000000000000000000000000000000000000ffffffff580426efcf00042d133b610cfabe6d6d4b6e5eda06373bc46000cf601b661f9243db16f482156a91d7c9008ca9a1d4c501000000000000000a2a6172e08a7c0100000a6b14363931303037343761363664363235343236363600000000020000000000000000266a24aa21a9ed47542e50fcec7a7b535c2b6a35cf4ac18722ad3289d9b90d037124862db8ec3e6c9770930b0000001976a9146f5cf1be10f3ad794eca67821fb280305b2900a188ac0120000000000000000000000000000000000000000000000000000000000000000000000000", "privkeys", null, "ALL"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": null
}
```
