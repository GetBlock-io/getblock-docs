---
description: >-
  Example code for the get_random_outputs json-rpc method. Сomplete guide on how
  to use get_random_outputs json-rpc in GetBlock.io Web3 documentation.
---

# get\_random\_outputs - Bitcoin Cash

#### Parameters

`amounts` - array of uint64

Amounts in Atomic Units in an Array.

`output_count` - uint32

Number of outputs to show for each specified amount in amounts.

`confirmed_height_or_depth` - int32

Optional.

Positive values are read as height, negative values are read as depth\*, 0 is genesis block. Mix-ins will be selected from the \[0..confirmed\_height\_or\_depth] window.

\* Depth is calculated from the tip block. Different nodes may have different tip numbers depending on blockchain sync status.

#### Request

```java
curl --location --request POST 'https://bcn.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "get_random_outputs",
"params": {"amounts": [100, 200], "output_count": 2, "confirmed_height_or_depth": -1},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "outputs": {
            "100": [
                {
                    "amount": 100,
                    "global_index": 63491465,
                    "height": 2400148,
                    "public_key": "11e1c823ce6e0c6aecca65917353db4988b53f91f3888ebb84c729927d929e3d",
                    "stack_index": 18560,
                    "unlock_block_or_timestamp": 0,
                    "unlock_time": 0
                },
                {
                    "amount": 100,
                    "global_index": 61165945,
                    "height": 2143330,
                    "public_key": "a4a5b245313d5772bce4fd7d9e7d4d390d6d7a42cf96a7ca2bcc5ec615ab84e9",
                    "stack_index": 18050,
                    "unlock_block_or_timestamp": 0,
                    "unlock_time": 0
                }
            ],
            "200": [
                {
                    "amount": 200,
                    "global_index": 63131082,
                    "height": 2350507,
                    "public_key": "53485a8c19eaa4788e6dbd5c455e238699c111e4e4019fb0a53fa955424e6ff1",
                    "stack_index": 16953,
                    "unlock_block_or_timestamp": 0,
                    "unlock_time": 0
                },
                {
                    "amount": 200,
                    "global_index": 57876995,
                    "height": 1955723,
                    "public_key": "96c92f1ca73bd7d8a6e2b6f2bdbbcb15f892cefb4b7c90e0baf9d8f112186e0d",
                    "stack_index": 13438,
                    "unlock_block_or_timestamp": 0,
                    "unlock_time": 0
                }
            ]
        }
    }
}
```
