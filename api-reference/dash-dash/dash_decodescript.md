---
description: >-
  Example code for the decodescript json-rpc method. Сomplete guide on how to
  use decodescript json-rpc in GetBlock.io Web3 documentation.
---

# decodescript - Dash

#### Parameters

`Redeem Script` - string (hex)

The redeem script to decode as a hex-encoded serialized script.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "decodescript",
"params": ["76a9147c086eada12bdb10a265c16c08a7ae87366bd48188ac"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "addresses": [
            "XmzfivrzYQ7B7oBMZKwPRdhjB1iNvX71XZ"
        ],
        "asm": "OP_DUP OP_HASH160 7c086eada12bdb10a265c16c08a7ae87366bd481 OP_EQUALVERIFY OP_CHECKSIG",
        "p2sh": "7n9YufoWBrJ65YC7sgwk53EzbPHMQRB6HJ",
        "reqSigs": 1,
        "type": "pubkeyhash"
    }
}
```
