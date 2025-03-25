---
description: >-
  Example code for the /chain/last-block rest method. Сomplete guide on how to
  use /chain/last-block rest in GetBlock.io Web3 documentation.
---

# /chain/last-block - NEM

#### Parameters

\-

#### Request

```java
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/chain/last-block' \
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "height": 3943119,
    "prevBlockHash": {
        "data": "cbb6d1f9ee968e8605e50cdd5fb17bf2ff4ad60ddb486d136500a67b6dcd12dd"
    },
    "signature": "4a4f21315eec555d61b01aa1f5ae607bc664dae9be6821d06aeb3b3e0d65712932755d435b14e0a3b43d34c113477bca6f72160cbde5c73b44a6ee9765e80a05",
    "signer": "47ff934888ed8ea66433c889630bf67a46d47717e99e2f29db5b3866e7cc4c89",
    "timeStamp": 238621889,
    "transactions": [],
    "type": 1,
    "version": 1744830465
}
```
