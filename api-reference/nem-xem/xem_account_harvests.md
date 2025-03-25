---
description: >-
  Example code for the /account/harvests rest method. Сomplete guide on how to
  use /account/harvests rest in GetBlock.io Web3 documentation.
---

# /account/harvests - NEM

#### Parameters

`address` -

The address of the account.

`hash` -

The 256 bit sha3 hash of the block up to which harvested blocks are returned.

#### Request

```java
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/account/harvests?address=NCXIQA4FF5JB6AMQ53NQ3ZMRD3X3PJEWDJJJIGHT&hash=ff8f8d88a65d499165c2e4fa1c95bd4d5366f71d7a62efc21b0df39b6c80613a' \
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "data": [
        {
            "difficulty": 22388850229680,
            "height": 3943916,
            "id": 3944044,
            "timeStamp": 238670475,
            "totalFee": 0
        },
        {
            "difficulty": 21579979856892,
            "height": 3943904,
            "id": 3944032,
            "timeStamp": 238669743,
            "totalFee": 0
        }
    ]
}
```
