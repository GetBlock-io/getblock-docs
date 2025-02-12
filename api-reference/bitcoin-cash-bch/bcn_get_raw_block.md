---
description: >-
  Example code for the get_raw_block json-rpc method. Сomplete guide on how to
  use get_raw_block json-rpc in GetBlock.io Web3 documentation.
---

# get\_raw\_block - Bitcoin Cash

#### Parameters

`hash` - string

Optional.

Hash of a block to get.

`height_or_depth` - int32

Optional.

Height or depth of a block to get.

#### Request

```java
curl --location --request POST 'https://bcn.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "get_raw_block",
"params": {"height_or_depth": -1},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "block": {
            "header": {
                "already_generated_coins": 18444686308329240679,
                "already_generated_key_outputs": 63518445,
                "already_generated_transactions": 6665322,
                "base_reward": 7849781914,
                "binary_nonce": "a8c06108",
                "block_capacity_vote": 100000,
                "block_capacity_vote_median": 100000,
                "block_size": 624,
                "cumulative_difficulty": 30676358330795090,
                "difficulty": 10361588328,
                "effective_size_median": 0,
                "hash": "5099fb0b04b83d4f9862109608349d7a25938e6aa00b509366848589fe69917a",
                "height": 2403323,
                "major_version": 4,
                "minor_version": 7,
                "nonce": 140624040,
                "previous_block_hash": "bb98a8b435741b08a2d77700fc8c08a38d019f00bda4ccf96bee73965bac59ec",
                "reward": 7849781914,
                "size_median": 0,
                "timestamp": 1631799662,
                "timestamp_median": 1631795442,
                "transactions_fee": 0,
                "transactions_size": 269
            },
            "output_stack_indexes": [
                [
                    1637,
                    945,
                    1415177,
                    792233,
                    881336,
                    859122
                ]
            ],
            "raw_header": {
                "coinbase_transaction": {
                    "extra": "01f6e7bc178e5509cda58a22e5c246ed9d498f40982d01f3ab765cde305c4fa4a40403a08d06",
                    "inputs": [
                        {
                            "height": 2403323,
                            "type": "coinbase"
                        }
                    ],
                    "outputs": [
                        {
                            "amount": 781000,
                            "public_key": "ac1b77f4d5956e3ad137428b0a6956e5f99d63b813a54e898be31b0dc18c5af0",
                            "type": "key"
                        },
                        {
                            "amount": 914,
                            "public_key": "58d0f884ed24bf0f7bb5e77aa5d4a33a324ad82c752567bee271918ec85c6c28",
                            "type": "key"
                        },
                        {
                            "amount": 9000000,
                            "public_key": "0babba074029d95c979dad8084058042d3e55aa7d86def9eb395748a7fdd836d",
                            "type": "key"
                        },
                        {
                            "amount": 40000000,
                            "public_key": "3446c6d970ba9b44b7c27d78525e1de7181d7dbb7e3a28d6bb80c6db4062922d",
                            "type": "key"
                        },
                        {
                            "amount": 800000000,
                            "public_key": "d30bd5c88b876e6812fc6c49bf5c3a33c3ac1ea88723affe7d08f1b4f8a7b687",
                            "type": "key"
                        },
                        {
                            "amount": 7000000000,
                            "public_key": "e0d04b3335813afd01cb3f4f2188173a136acf72ed8a5b7f00310ffaba0044cb",
                            "type": "key"
                        }
                    ],
                    "unlock_block_or_timestamp": 0,
                    "version": 1
                },
                "major_version": 4,
                "minor_version": 7,
                "previous_block_hash": "bb98a8b435741b08a2d77700fc8c08a38d019f00bda4ccf96bee73965bac59ec",
                "root_block": {
                    "blockchain_branch": [],
                    "coinbase_transaction": {
                        "extra": "016e2f724814e3fa66fe920703bcb922ec11898b7d0046f9cec1de1e2dca0b5c3202110000015a809fb18366ff36e40000000000032100d04880c60f277f3ffe6867890b208eb99c22ce6d78530f8d4b0a611d6c11d644",
                        "ignored": 0,
                        "inputs": [
                            {
                                "height": 2458620,
                                "type": "coinbase"
                            }
                        ],
                        "outputs": [
                            {
                                "amount": 3001568040000,
                                "public_key": "1e8761db844f70b99ebbcdf2ce64b650a6062f59f19b89c063922f4fcc0b8749",
                                "type": "key"
                            },
                            {
                                "amount": 7003658760000,
                                "public_key": "7ad0a33366d2954da065e2dfcb40dd41b867927d813d848ef8eaf92a744f8253",
                                "type": "key"
                            }
                        ],
                        "unlock_block_or_timestamp": 2458680,
                        "version": 2
                    },
                    "coinbase_transaction_branch": [
                        "82f78d91d95e716136678fc93b457338b880e025c7de6d78bcda3ac325b24cf3",
                        "54d42069cc0c25824f95e3560c633e07b9c4e951f98032104038dd67d5d8e973",
                        "31ffb373c8b5efd5f1a84a042274bbb742cd38a3f7f3b3658924268fd4501e07"
                    ],
                    "major_version": 60,
                    "minor_version": 60,
                    "nonce": "a8c06108",
                    "previous_block_hash": "e8f418d8ec730f44b9a466203489226fc5dd3a031b87dcb5b49d66671de62dd1",
                    "timestamp": 1631799662,
                    "transaction_count": 8
                },
                "transaction_hashes": []
            },
            "raw_transactions": [],
            "transactions": [
                {
                    "amount": 7849781914,
                    "anonymity": 0,
                    "binary_size": 269,
                    "block_hash": "5099fb0b04b83d4f9862109608349d7a25938e6aa00b509366848589fe69917a",
                    "block_height": 2403323,
                    "coinbase": true,
                    "extra": "01f6e7bc178e5509cda58a22e5c246ed9d498f40982d01f3ab765cde305c4fa4a40403a08d06",
                    "fee": 0,
                    "hash": "7dcc08d3c7271693b2be5e0d467e151546b28e9cc961d117974a85b739daf188",
                    "inputs_hash": "3f25c64e0ccac4bb4b9e05b1400bc1f75e04abbc9b7fefe97ae35fc5b496b458",
                    "prefix_hash": "7dcc08d3c7271693b2be5e0d467e151546b28e9cc961d117974a85b739daf188",
                    "public_key": "f6e7bc178e5509cda58a22e5c246ed9d498f40982d01f3ab765cde305c4fa4a4",
                    "size": 269,
                    "timestamp": 1631799662,
                    "unlock_block_or_timestamp": 0,
                    "unlock_time": 0
                }
            ]
        },
        "depth": -1,
        "orphan_status": false
    }
}
```
