---
description: >-
  Example code for the getrawmempool json-rpc method. Сomplete guide on how to
  use getrawmempool json-rpc in GetBlock.io Web3 documentation.
---

# getrawmempool - DigiByte

#### Parameters

`verbose` - boolean, optional, default=false

True for a json object, false for array of transaction ids

`mempool_sequence` - boolean, optional, default=false

If verbose=false, returns a json object with transaction list and mempool sequence number attached.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getrawmempool",
"params": [false, false],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": [
        "14ae057536076b7d03cad92fef281ac6f4586c07c0d4295b8107003f973bd58b",
        "dd728cf03ab6b19ab4c7530257c3e34f11c03376f63756e7df132cad38704c72",
        "083dacffb7227bfc980b35ca03d378a7cf7ec4465b69e945db22f12b962c9fc4",
        "282a100a832a7be28cd0e72a794b6314fc1e2d14f4f20b21be6c92c1f9049181",
        "fde610d606c9ff9e9641aa9c9d4b276f235fc58299a73b26be43d955910051bc",
        "fd03a2fec259f05fe6e37b18dc879c5b0027dede465f427fd78fb9da7f1339c9",
        "9a3cc5bb9bfd5b68090c054fdf007b3bc582e68d895070c9d89223352d19a69b"
    ]
}
```
