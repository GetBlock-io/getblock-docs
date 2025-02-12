---
description: >-
  Example code for the /account/get/forwarded/from-public-key rest method.
  Сomplete guide on how to use /account/get/forwarded/from-public-key rest in
  GetBlock.io Web3 documentation.
---

# /account/get/forwarded/from-public-key - NEM

#### Parameters

`publicKey` -

The public key of the account as hex string.

#### Request

```java
curl --location --request GET 'https://xem.getblock.io/account/get/forwarded/from-public-key?publicKey=107051c28a2c009a83ae0861cdbff7c1cbab387c964cc433f7d191d9c3115ed7'
 --header 'x-api-key: YOUR-API-KEY' 
 --header 'Content-Type: application/json'
```

#### Response

```java
{
    "account": {
        "address": "NCXIQA4FF5JB6AMQ53NQ3ZMRD3X3PJEWDJJJIGHT",
        "balance": 20612640711967,
        "harvestedBlocks": 52431,
        "importance": 0.0019859907077256836,
        "label": null,
        "multisigInfo": {},
        "publicKey": "107051c28a2c009a83ae0861cdbff7c1cbab387c964cc433f7d191d9c3115ed7",
        "vestedBalance": 20612602402832
    },
    "meta": {
        "cosignatories": [],
        "cosignatoryOf": [],
        "remoteStatus": "ACTIVE",
        "status": "LOCKED"
    }
}
```
