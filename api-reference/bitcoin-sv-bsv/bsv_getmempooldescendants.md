---
description: >-
  Example code for the getmempooldescendants json-rpc method. Сomplete guide on
  how to use getmempooldescendants json-rpc in GetBlock.io Web3 documentation.
---

# getmempooldescendants - Bitcoin SV

#### Parameters

`txid` - string, required

The transaction id (must be in mempool)

`verbose` - boolean, optional, default=false

True for a json object, false for array of transaction ids

#### Request

```java
curl --location --request POST 'https://bsv.getblock.io/mainnet/' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getmempooldescendants",
"params": ["a4aef4dd6721ff3e39a6f9b55d87ec2b72bc5bb55ea806ba75aa6c27b2a335df", null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": [
        "d8047111681bcb24ade1355458bee569dfa2eda33ddf0d6a3b1eaa21fca0729b",
        "c7229dcd13432d3dafb8408eb4c61c6973b585b4232039ff2a777057cd3f419f",
        "ebfa35ff32bacd2e2f31fa09b77a5baf0dd04ea69b49731c785fa14cb4ae26c6",
        "177b36ad581a51d9eaaa23d6933ee932a1c4997d12469b9ed6ea3ec83ae626c8",
        "ca603f623a08bd0e825792de6ee158d3a92af4ba512cc9202dd17af6be35dcd3",
        "65ac887d001640b4a1e490f315af37f02a8962105f0e364429879d55e967c1e2",
        "f20c3ba1ee5dd284fa75758ff3bdd2805323f6dd590f5ffe27892c672f0f8ff3",
        "7b28bf17e2757475dc5ff3709dae1d70fc1624eecc0118f5023c49c2b8db08f7"
    ]
}
```
