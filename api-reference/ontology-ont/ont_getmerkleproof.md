---
description: >-
  Example code for the getmerkleproof json-rpc method. Сomplete guide on how to
  use getmerkleproof json-rpc in GetBlock.io Web3 documentation.
---

# getmerkleproof - Ontology

#### Parameters

`hash` - string

transaction hash

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getmerkleproof",
"params": ["ba3607c52b76d1fe93fa032bc74212c01139d06702f6e3f9214692a2077b1aa1"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "desc": "SUCCESS",
    "error": 0,
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "BlockHeight": 16224208,
        "CurBlockHeight": 16224577,
        "CurBlockRoot": "24846ffb7adea6679cc439e914a3f2d1d17b3771b04386e8ed73897df8daff2b",
        "TargetHashes": [
            "0000000000000000000000000000000000000000000000000000000000000000",
            "dfce4b5592f00840b0b92767a71d9def22f775d2f6bbb5bd51e8e8f9eb0067d7",
            "d149438da06747bec63bbbe21ae605d080383cfb79cb6456de99629c875b8bd1",
            "b37b061129c773c083e93682312af4ba12ffbe903c820c6372a6ca661a686e44",
            "16a4b98f501ec1178ad5e37d211d69a62bdc0e32974fff20f32f7558f8658806",
            "d157441a1ea3927e0cc5fb5a5c7e6a161a59e5bd83cee53eba78500be65e46bf",
            "8fe14bca99660c21a5f29ea73090978114f3c27fdacff1a869ace26f99ef4fa2"
        ],
        "TransactionsRoot": "ba3607c52b76d1fe93fa032bc74212c01139d06702f6e3f9214692a2077b1aa1",
        "Type": "MerkleProof"
    }
}
```
