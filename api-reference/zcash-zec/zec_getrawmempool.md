---
description: >-
  Example code for the getrawmempool json-rpc method. Сomplete guide on how to
  use getrawmempool json-rpc in GetBlock.io Web3 documentation.
---

# getrawmempool - Zcash

#### Parameters

`verbose` - boolead

Optional, default=false.

true for a json object, false for array of transaction ids.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getrawmempool",
"params": [false],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": [
        "e49ff2154e6694a5523f6ab696b2150ecd76e6726f31bf4c18fdb4ca82dbec2d",
        "4643018d74e057ce4040d9a13cd0d2b15f770747acc5aa6bd727a06b2993daee",
        "a52ef3869dde4f7cf8347149debf0e306e91b569ec75d4508d864cbd8a00002f",
        "3c049f8253480c188e80dde21dff3d549818243c4212313e76842a00315bcdf9",
        "306dcc5bdaf058f48edbd75909a8c13b5813a8c4fc8419fb0a96595167fa17b1",
        "71d4a08f72922c700cee00ea184cfa2be10a707c97b78d60fecbbbe95146a418",
        "1cf0b8e6b0fd8f406a22d05abf95e61e93977f8710905c7a1a44e78741989da5",
        "31a3a2ab6290672d3c4061bcc7fc3be3f84ef1bc4e438605012e1857f529b459"
    ]
}
```
