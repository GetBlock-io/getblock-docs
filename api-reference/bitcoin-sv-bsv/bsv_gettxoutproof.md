---
description: >-
  Example code for the gettxoutproof json-rpc method. Сomplete guide on how to
  use gettxoutproof json-rpc in GetBlock.io Web3 documentation.
---

# gettxoutproof - Bitcoin SV

#### Parameters

`txids` - json array, required

A json array of txids to filter

`blockhash` - string, optional

If specified, looks for txid in the block with this hash

#### Request

```java
curl --location --request POST 'https://bsv.getblock.io/mainnet/' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "gettxoutproof",
"params": [["ee652f0b40209bd02468de0c6336854f5efdd79fb865560aef2c46f4fa0b4a07", "cee11bc3bb3d9db8c4813ed2072a14369a15fcfb9e6bc5cb37a0b5bcc6aa59aa"], null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": "000000202834abd71bdd0d3298542af4506918ea168ce002936b040000000000000000001da8757e4d756e848245cacf3e103c1b9f6ed2405c6d818a73172c8ec72856d4db3864606fdf0c17dcc1000ccb0300000cb86343fc64abcdab51e530303a4ee2b420fa6b5a12b435c9c76fe953ca5471ca074a0bfaf4462cef0a5665b89fd7fd5e4f8536630cde6824d09b20400b2f65eed9f744b2dc695b0ea0c4afd06310a21b93ddd7270a781acd0ada1afdd23b5750aa59aac6bcb5a037cbc56b9efbfc159a36142a07d23e81c4b89d3dbbc31be1cefe0bb7b0369ffc3b1d530e234987543a2613bbb8b06c86f993a930dee7b9d87f661ef556adc0174c7f180aa28006ee93ce2291302801ecd045c234c00b186ea35ff1e77eac3f113492e2eb12f38b9df452f5831f55c861865ac8f3c7dd06be2377f859ba1d12dea2ec44987796a27d42d5727250c1e0181d6a251f8272f21b9a2034069a2471de43de655619904d43b4665f6ce38741320998dc97838c32c79f1ada066ddf7a441357d55cc42a8906970bff2d5342be694002476733ff593af26f320c10df7ba9a76355438f462c040b598868dfb67c5e88d6d9a426ec8cdd74337d42df6b29e9fb319410848f3ff7228d00dc539e2962d185348ab9663a112a03ff6e00"
}
```
