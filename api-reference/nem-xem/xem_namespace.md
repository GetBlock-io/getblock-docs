---
description: >-
  Example code for the /namespace rest method. Сomplete guide on how to use
  /namespace rest in GetBlock.io Web3 documentation.
---

# /namespace - NEM

#### Parameters

`namespace` -

The namespace id.

#### Request

```java
curl --location --request GET 'https://xem.getblock.io/namespace?namespace=cactus'  
 --header 'x-api-key: YOUR-API-KEY' 
 --header 'Content-Type: application/json'
```

#### Response

```java
{
    "fqn": "cactus",
    "height": 3915451,
    "owner": "NDOQZVUH5ZZHPFOFFIF3QYIDU2CRVZOAH5DAOCF6"
}
```
