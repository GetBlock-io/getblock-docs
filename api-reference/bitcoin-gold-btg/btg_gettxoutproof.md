---
description: >-
  Example code for the gettxoutproof json-rpc method. Сomplete guide on how to
  use gettxoutproof json-rpc in GetBlock.io Web3 documentation.
---

# gettxoutproof - Bitcoin Gold

#### Parameters

`txids` - json array, required

A json array of txids to filter

`blockhash` - string, optional

If specified, looks for txid in the block with this hash

#### Request

```java
curl --location --request POST 'https://btg.getblock.io/mainnet' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "gettxoutproof",
"params": [["5c970c064d317658a8777ee56f192fe17d5b3afbe242d820b205c2291210f269"], null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": "000000203bf834f7a8f5cd2b75c3a224e9bf3924b4f4b4daea753dd8d59055f20000000069f2101229c205b220d842e2fb3a5b7de12f196fe57e77a85876314d060c975c36b80a000000000000000000000000000000000000000000000000000000000083383661c964031d040046f60000000000000000000000000000000000000000000000003a9c0000640c8906341016d6e8e8d1fc1f85493065a530ea0f3bbbbe2aa52326f6d4c06920bcbc35b77657b06ef7073f4e28512ff593480d476bcd5bb44c1e510f0af2a525a002f85515683d9d2dcfee0f669e1b133d0b9610bb3ac1043c45262fc811c75b8170e1c5010000000169f2101229c205b220d842e2fb3a5b7de12f196fe57e77a85876314d060c975c0101"
}
```
