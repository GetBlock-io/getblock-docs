---
description: >-
  Example code for the /account/mosaic/owned rest method. Сomplete guide on how
  to use /account/mosaic/owned rest in GetBlock.io Web3 documentation.
---

# /account/mosaic/owned - NEM

#### Parameters

`address` -

The address of the account.

#### Request

```java
curl --location --request GET 'https://xem.getblock.io/account/mosaic/owned?address=NCXIQA4FF5JB6AMQ53NQ3ZMRD3X3PJEWDJJJIGHT'
 --header 'x-api-key: YOUR-API-KEY' 
 --header 'Content-Type: application/json'
```

#### Response

```java
{
    "data": [
        {
            "mosaicId": {
                "name": "xem",
                "namespaceId": "nem"
            },
            "quantity": 20612640711967
        },
        {
            "mosaicId": {
                "name": "ch0459804891b",
                "namespaceId": "asset"
            },
            "quantity": 2000000
        }
    ]
}
```
