---
description: >-
  Example code for the gobject_get json-rpc method. Сomplete guide on how to use
  gobject_get json-rpc in GetBlock.io Web3 documentation.
---

# gobject\_get - Dash

#### Parameters

`method name` - string

None

`governance-hash` - string (hex)

The hash of a governance object

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "gobject",
"params": ["get", "1f8ae9f6e435a22ce74139997bad70384a84452f95c5054a2c76602609ddc700"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "CollateralHash": "19cf17c8ba095f90ab22f9c22f950b100b64f8429571bf5f502cf3d57169b753",
        "CreationTime": 1627652440,
        "DataHex": "7b22656e645f65706f6368223a313633393039383431352c226e616d65223a224272617a696c513432303231222c227061796d656e745f61646472657373223a225871746671736f44663254376e636842394e55716b71796b766f777a51684c684150222c227061796d656e745f616d6f756e74223a3131302c2273746172745f65706f6368223a313632383739363439352c2274797065223a312c2275726c223a2268747470733a2f2f7777772e6461736863656e7472616c2e6f72672f702f446173684272617a696c513432303231227d",
        "DataString": "{\"end_epoch\":1639098415,\"name\":\"BrazilQ42021\",\"payment_address\":\"XqtfqsoDf2T7nchB9NUqkqykvowzQhLhAP\",\"payment_amount\":110,\"start_epoch\":1628796495,\"type\":1,\"url\":\"https://www.dashcentral.org/p/DashBrazilQ42021\"}",
        "DeleteResult": {
            "AbsoluteYesCount": 2,
            "AbstainCount": 0,
            "NoCount": 0,
            "YesCount": 2
        },
        "EndorsedResult": {
            "AbsoluteYesCount": 0,
            "AbstainCount": 0,
            "NoCount": 0,
            "YesCount": 0
        },
        "FundingResult": {
            "AbsoluteYesCount": 531,
            "AbstainCount": 42,
            "NoCount": 185,
            "YesCount": 716
        },
        "Hash": "1f8ae9f6e435a22ce74139997bad70384a84452f95c5054a2c76602609ddc700",
        "IsValidReason": "",
        "ObjectType": 1,
        "ValidResult": {
            "AbsoluteYesCount": 0,
            "AbstainCount": 0,
            "NoCount": 0,
            "YesCount": 0
        },
        "fCachedDelete": false,
        "fCachedEndorsed": false,
        "fCachedFunding": true,
        "fCachedValid": true,
        "fLocalValidity": true
    }
}
```
