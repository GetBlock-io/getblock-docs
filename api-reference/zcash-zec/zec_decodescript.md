---
description: >-
  Example code for the decodescript json-rpc method. Сomplete guide on how to
  use decodescript json-rpc in GetBlock.io Web3 documentation.
---

# decodescript - Zcash

#### Parameters

`hex` - string

The hex-encoded script.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "decodescript",
"params": ["76a91417b04a8ede7164eccb961f46289305ec04014b6388ac"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "addresses": [
            "t1L2rjgGrvEqfrA5zqUca4GGxAeQg47CTpG"
        ],
        "asm": "OP_DUP OP_HASH160 17b04a8ede7164eccb961f46289305ec04014b63 OP_EQUALVERIFY OP_CHECKSIG",
        "p2sh": "t3V5ob2dTxoQRsZXZdBPMf7Ztori9Z86Vcd",
        "reqSigs": 1,
        "type": "pubkeyhash"
    }
}
```
