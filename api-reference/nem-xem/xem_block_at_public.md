---
description: >-
  Example code for the /block/at/public rest method. Сomplete guide on how to
  use /block/at/public rest in GetBlock.io Web3 documentation.
---

# /block/at/public - NEM

#### Parameters

`height` -

A BlockHeight JSON object. The block height describes the position of the block within the block chain. The first block of the chain has height one. Each subsequent block has a height which is one higher than the previous block.

#### Request

```java
curl --location --request POST 'https://xem.getblock.io/block/at/public'
 --header 'x-api-key: YOUR-API-KEY'
 --header 'Content-Type: application/json'
 --data-raw '{'height':394311}'
```

#### Response

```java
{
    "timeStamp": 23938014,
    "signature": "7756ee2ba07a4ae4563520ae7049ee3d7ecd78b3ae69b3401c4a956aa99143b24257479c525cc1d89e4aaf2bf2e7be879b158a5cff80827b94ca6f3e8fb1af0c",
    "prevBlockHash": {
        "data": "d88719d3b875894f5d138b72b645a61139c887a5014eadc45aa6d075a8ba6cd0"
    },
    "type": 1,
    "transactions": [],
    "version": 1744830465,
    "signer": "d758fea02f28cf788f3bcd408241c09088eff358305ff613dceecaaa851f32ef",
    "height": 394311
}
```
