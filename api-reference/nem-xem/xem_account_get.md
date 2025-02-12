---
description: >-
  Example code for the /account/get rest method. Сomplete guide on how to use
  /account/get rest in GetBlock.io Web3 documentation.
---

# /account/get - NEM

#### Parameters

`address` -

The address of the account.

#### Request

```java
curl --location --request GET 'https://xem.getblock.io/account/get?address=NC4T246ALCPNBTAOCSC5EAVFMDFBOACSQAF6WKHV'
 --header 'x-api-key: YOUR-API-KEY'
 --header 'Content-Type: application/json'
```

#### Response

```java
{
    "account": {
        "address": "NC4T246ALCPNBTAOCSC5EAVFMDFBOACSQAF6WKHV",
        "balance": 0,
        "harvestedBlocks": 0,
        "importance": 0.0,
        "label": null,
        "multisigInfo": {},
        "publicKey": null,
        "vestedBalance": 0
    },
    "meta": {
        "cosignatories": [],
        "cosignatoryOf": [],
        "remoteStatus": "INACTIVE",
        "status": "LOCKED"
    }
}
```
