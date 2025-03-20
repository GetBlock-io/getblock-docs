---
description: >-
  Example code for the /v1/accounts/{account_hash}/resources json-rpc method.
  Сomplete guide on how to use /v1/accounts/{account_hash}/resources json-rpc in
  GetBlock.io Web3 documentation.
---

# /v1/accounts/{account\_hash}/resources - Aptos

#### Parameters

`limit` -

Maximum number of resources to retrieve. Gets default page size if not provided.

`ledger_version` -

Ledger version. Defaults to latest if not provided.

`start` -

Optional cursor specifying pagination start. You can call this endpoint once without this parameter, and then use the cursor returned in the X-Aptos-Cursor header in the response.

#### Request

```java
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/v1/accounts/0xc20ea5a196c81d8d7aff814aa37f8a5823acffbc4193efd3b2aafc9ef2803255/resources?limit=10' \
--header 'Content-Type: application/json'
```

#### Response

```java
[
    {
        "data": {
            "authentication_key": "0xc20ea5a196c81d8d7aff814aa37f8a5823acffbc4193efd3b2aafc9ef2803255",
            "coin_register_events": {
                "counter": "1",
                "guid": {
                    "id": {
                        "addr": "0xc20ea5a196c81d8d7aff814aa37f8a5823acffbc4193efd3b2aafc9ef2803255",
                        "creation_num": "0"
                    }
                }
            },
            "guid_creation_num": "4",
            "key_rotation_events": {
                "counter": "0",
                "guid": {
                    "id": {
                        "addr": "0xc20ea5a196c81d8d7aff814aa37f8a5823acffbc4193efd3b2aafc9ef2803255",
                        "creation_num": "1"
                    }
                }
            },
            "rotation_capability_offer": {
                "for": {
                    "vec": []
                }
            },
            "sequence_number": "497660",
            "signer_capability_offer": {
                "for": {
                    "vec": []
                }
            }
        },
        "type": "0x1::account::Account"
    },
    {
        "data": {
            "coin": {
                "value": "42470628935"
            },
            "deposit_events": {
                "counter": "2",
                "guid": {
                    "id": {
                        "addr": "0xc20ea5a196c81d8d7aff814aa37f8a5823acffbc4193efd3b2aafc9ef2803255",
                        "creation_num": "2"
                    }
                }
            },
            "frozen": false,
            "withdraw_events": {
                "counter": "1",
                "guid": {
                    "id": {
                        "addr": "0xc20ea5a196c81d8d7aff814aa37f8a5823acffbc4193efd3b2aafc9ef2803255",
                        "creation_num": "3"
                    }
                }
            }
        },
        "type": "0x1::coin::CoinStore<0x1::aptos_coin::AptosCoin>"
    }
]
```
