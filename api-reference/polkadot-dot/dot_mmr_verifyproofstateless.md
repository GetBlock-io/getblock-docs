---
description: >-
  Example code for the mmr_verifyProofStateless  {disallowed} json-rpc method.
  Сomplete guide on how to use mmr_verifyProofStateless  {disallowed} json-rpc
  in GetBlock.io Web3 documentation.
---

# mmr\_verifyProofStateless {disallowed} - Polkadot

#### Parameters

`root` - MmmrHash

mmr hash

`proof` - MmmLeafBatchProof

mmr leaf batch proof

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "mmr_verifyProofStateless",
"params": [null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```
