---
description: >-
  Example code for the eth_getAssetBalance  {disallowed} json-rpc method.
  Сomplete guide on how to use eth_getAssetBalance  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_getAssetBalance {disallowed} - Avalanche

#### Parameters

`address` - string

owner of the asset

`blk` - blkNrOrHash

blk is the block number or hash at which to retrieve the balance

`assetID` - string

id of the asset for which the balance is requested

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/mainnet/ext/bc/C/rpc' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "eth_getAssetBalance",
  "params": [
    "0x8723e5773847A4Eb5FeEDabD9320802c5c812F46",
    "latest",
    "3RvKBAmQnfYionFXMfW5P8TDZgZiogKbHjM8cjpu16LKAgF5T"
  ],
  "id": "getblock.io"
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
