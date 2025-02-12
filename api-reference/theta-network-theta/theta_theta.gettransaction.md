---
description: >-
  Example code for the theta.GetTransaction json-rpc method. Сomplete guide on
  how to use theta.GetTransaction json-rpc in GetBlock.io Web3 documentation.
---

# theta.GetTransaction - Theta Network

#### Parameters

`hash` - string

The transaction hash.

#### Request

```java
curl --location --request POST 'https://theta.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "theta.GetTransaction",
"params": {"hash": "0x380a2648e772ed7e0460a4a6711d2f0f9fee842450a7ff7c289655d72b76ef9b"},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "block_hash": "0x571e11a52a269f2f3f6eae033a8889c66f77a9855fb52bc5a89d05b13f8fd77c",
        "block_height": "11885134",
        "hash": "0x380a2648e772ed7e0460a4a6711d2f0f9fee842450a7ff7c289655d72b76ef9b",
        "receipt": null,
        "status": "finalized",
        "transaction": {
            "block_height": "11885133",
            "outputs": [],
            "proposer": {
                "address": "0x4b80a68a8469d33449eb101082e5500b932a23ce",
                "coins": {
                    "tfuelwei": "0",
                    "thetawei": "0"
                },
                "sequence": "0",
                "signature": "0x72834f1e57f03ad4b623cc425b073e2bd16ba9ccc309f7ad3d127d03a4decf3243d3b7ea4d6e61221ac050d21593518d71025cf54a2e63f8fc31ee8f77e498e701"
            }
        },
        "type": 0
    }
}
```
