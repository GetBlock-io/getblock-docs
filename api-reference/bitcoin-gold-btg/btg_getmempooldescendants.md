---
description: >-
  Example code for the getmempooldescendants json-rpc method. Сomplete guide on
  how to use getmempooldescendants json-rpc in GetBlock.io Web3 documentation.
---

# getmempooldescendants - Bitcoin Gold

#### Parameters

`txid` - string, required

The transaction id (must be in mempool)

`verbose` - boolean, optional, default=false

True for a json object, false for array of transaction ids

#### Request

```java
curl --location --request POST 'https://btg.getblock.io/mainnet' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
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
        "c7229dcd13432d3dafb8408eb4c61c6973b585b4232039ff2a777057cd3f419f",
        "d8047111681bcb24ade1355458bee569dfa2eda33ddf0d6a3b1eaa21fca0729b",
        "177b36ad581a51d9eaaa23d6933ee932a1c4997d12469b9ed6ea3ec83ae626c8",
        "ca603f623a08bd0e825792de6ee158d3a92af4ba512cc9202dd17af6be35dcd3"
    ]
}
```
