---
description: >-
  Example code for the /namespace/root/page rest method. Сomplete guide on how
  to use /namespace/root/page rest in GetBlock.io Web3 documentation.
---

# /namespace/root/page - NEM

#### Parameters

`id` -

The topmost namespace database id up to which root namespaces are returned. The parameter is optional. If not supplied the most recent rented root namespaces are returned.

`pagesize` -

The number of namespace objects to be returned for each request. The parameter is optional. The default value is 25, the minimum value is 5 and hte maximum value is 100.

#### Request

```java
curl --location --request GET 'https://xem.getblock.io//namespace/root/page?id=26754&pagesize=5'
 --header 'x-api-key: YOUR-API-KEY' 
 --header 'Content-Type: application/json'
```

#### Response

```java
{
    "data": [
        {
            "meta": {
                "id": 5311
            },
            "namespace": {
                "fqn": "cloverclub",
                "height": 3800313,
                "owner": "NAMO2AOVUGYPBH6LIQRM3I227T6I6BXOQFYDAMFT"
            }
        },
        {
            "meta": {
                "id": 5310
            },
            "namespace": {
                "fqn": "beer-money",
                "height": 3800112,
                "owner": "NBRFW5P3FIXAWV7AOXE6EOEZLZWCBIWHPUG5UT6O"
            }
        }
    ]
}
```
