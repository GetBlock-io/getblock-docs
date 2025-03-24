---
description: >-
  Example code for the gobject_list json-rpc method. Сomplete guide on how to
  use gobject_list json-rpc in GetBlock.io Web3 documentation.
---

# gobject\_list - Dash

#### Parameters

`method name` - string

None

`signal` - string (hex)

Optional. Type of governance object signal:

\- valid

\- funding

\- delete

\- endorsed

\- all (default)

`type` - string (hex)

Optional. Type of governance object signal:

\- proposals

\- triggers

\- all (default)

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "gobject",
"params": ["list", null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "0aa6108f582a2f42479bcf500e2f69949cc4eafbf934d1616fbe0082cb167ce7": {
            "AbsoluteYesCount": 845,
            "AbstainCount": 0,
            "CollateralHash": "c1130cb867ed68aa5e724097da11f6d4005b76bcf3b8b89b807db9dc22b3f2b0",
            "CreationTime": 1629223668,
            "DataHex": "7b22656e645f65706f6368223a313633313339373338392c226e616d65223a224449462d4a554c59323032312d46554e44494e47222c227061796d656e745f61646472657373223a225876777034753743525a707a6f553772796b325136364a676f3932656a437662334b222c227061796d656e745f616d6f756e74223a3430302c2273746172745f65706f6368223a313632383832313930392c2274797065223a312c2275726c223a2268747470733a2f2f7777772e6461736863656e7472616c2e6f72672f702f4449462d4a756c79323032312d46554e44494e47227d",
            "DataString": "{\"end_epoch\":1631397389,\"name\":\"DIF-JULY2021-FUNDING\",\"payment_address\":\"Xvwp4u7CRZpzoU7ryk2Q66Jgo92ejCvb3K\",\"payment_amount\":400,\"start_epoch\":1628821909,\"type\":1,\"url\":\"https://www.dashcentral.org/p/DIF-July2021-FUNDING\"}",
            "Hash": "0aa6108f582a2f42479bcf500e2f69949cc4eafbf934d1616fbe0082cb167ce7",
            "IsValidReason": "",
            "NoCount": 42,
            "ObjectType": 1,
            "YesCount": 887,
            "fBlockchainValidity": true,
            "fCachedDelete": false,
            "fCachedEndorsed": false,
            "fCachedFunding": true,
            "fCachedValid": true
        },
        "0c55eb059c91d9b5810efa661b473d87b6984c2e9c9fee3542a7ae096929af23": {
            "AbsoluteYesCount": 849,
            "AbstainCount": 0,
            "CollateralHash": "3c9b6554c3d80587ed863d4aab24ff8d26039a667117e76d95a0e87d99340072",
            "CreationTime": 1628719172,
            "DataHex": "7b22656e645f65706f6368223a313633333936353631322c226e616d65223a22444347496e667261536570745f4f6374222c227061796d656e745f61646472657373223a225868766f524d4a4544596869736a676f715853335257334b3252426474444b625731222c227061796d656e745f616d6f756e74223a3534332c2273746172745f65706f6368223a313632383831343635322c2274797065223a312c2275726c223a2268747470733a2f2f7777772e6461736863656e7472616c2e6f72672f702f444347496e667261536570745f4f6374227d",
            "DataString": "{\"end_epoch\":1633965612,\"name\":\"DCGInfraSept_Oct\",\"payment_address\":\"XhvoRMJEDYhisjgoqXS3RW3K2RBdtDKbW1\",\"payment_amount\":543,\"start_epoch\":1628814652,\"type\":1,\"url\":\"https://www.dashcentral.org/p/DCGInfraSept_Oct\"}",
            "Hash": "0c55eb059c91d9b5810efa661b473d87b6984c2e9c9fee3542a7ae096929af23",
            "IsValidReason": "",
            "NoCount": 64,
            "ObjectType": 1,
            "YesCount": 913,
            "fBlockchainValidity": true,
            "fCachedDelete": false,
            "fCachedEndorsed": false,
            "fCachedFunding": true,
            "fCachedValid": true
        }
    }
}
```
