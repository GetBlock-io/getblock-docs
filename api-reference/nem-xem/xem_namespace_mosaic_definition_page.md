---
description: >-
  Example code for the /namespace/mosaic/definition/page rest method. Сomplete
  guide on how to use /namespace/mosaic/definition/page rest in GetBlock.io Web3
  documentation.
---

# /namespace/mosaic/definition/page - NEM

#### Parameters

`namespace` -

The namespace id.

`id` -

The topmost mosaic definition database id up to which root mosaic definitions are returned. The parameter is optional. If not supplied the most recent mosaic definitiona are returned.

`pagesize` -

The number of mosaic definition objects to be returned for each request. The parameter is optional. The default value is 25, the minimum value is 5 and hte maximum value is 100.

#### Request

```java
curl --location --request GET 'https://xem.getblock.io/namespace/mosaic/definition/page?namespace=cactus&pagesize=5}'
 --header 'x-api-key: YOUR-API-KEY' 
 --header 'Content-Type: application/json'
```

#### Response

```java
{
    "data": [
        {
            "meta": {
                "id": 3693
            },
            "mosaic": {
                "creator": "0e19f145b2e02f17d77283bede48463a6cac293ae89680cccf3d7c52be08117a",
                "description": "\u4eee\u60f3\u901a\u8ca8\u300ccactus\u300d\u306e\u30e2\u30b6\u30a4\u30af\u3067\u3059\u3002",
                "id": {
                    "name": "cactuscoin",
                    "namespaceId": "cactus"
                },
                "levy": {},
                "properties": [
                    {
                        "name": "divisibility",
                        "value": "6"
                    },
                    {
                        "name": "initialSupply",
                        "value": "100000000"
                    },
                    {
                        "name": "supplyMutable",
                        "value": "true"
                    },
                    {
                        "name": "transferable",
                        "value": "true"
                    }
                ]
            }
        }
    ]
}
```
