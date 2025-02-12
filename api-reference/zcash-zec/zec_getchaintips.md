---
description: >-
  Example code for the getchaintips json-rpc method. Сomplete guide on how to
  use getchaintips json-rpc in GetBlock.io Web3 documentation.
---

# getchaintips - Zcash

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getchaintips",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": [
        {
            "branchlen": 0,
            "hash": "00000000019af1612080b8d6b3ce87c5b7569a114e6a77385260cf45a6419fe1",
            "height": 1384358,
            "status": "active"
        },
        {
            "branchlen": 1,
            "hash": "0000000000b0f1c14cb0cf0c2754c27de40a290e692fb20ebca2fae9f599969f",
            "height": 1383875,
            "status": "valid-fork"
        },
        {
            "branchlen": 1,
            "hash": "000000000246b6cf748b89ddab52cd1c86618a87e47a1d82bc34c23f8a294f63",
            "height": 1383503,
            "status": "valid-fork"
        }
    ]
}
```
