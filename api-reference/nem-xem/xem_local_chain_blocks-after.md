---
description: >-
  Example code for the /local/chain/blocks-after  {disallowed} rest method.
  Сomplete guide on how to use /local/chain/blocks-after  {disallowed} rest in
  GetBlock.io Web3 documentation.
---

# /local/chain/blocks-after {disallowed} - NEM

#### Parameters

`height` -

A BlockHeight JSON object. The block height describes the position of the block within the block chain. The first block of the chain has height one. Each subsequent block has a height which is one higher than the previous block. The following structure is used by the NEM block chain explorer for convenience reason.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/YOUR-ACCESS-TOKEN/local/chain/blocks-after' \
--header 'Content-Type: application/json' \
--data-raw '{
    "height": 3943115
}'
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
