---
description: >-
  Example code for the sync_blocks json-rpc method. Сomplete guide on how to use
  sync_blocks json-rpc in GetBlock.io Web3 documentation.
---

# sync\_blocks - Bitcoin Cash

#### Parameters

`sparse_chain` - array of string

A specific sequence of local block hashes.

Sparse\_chain is a sequence of blocks hashes from the last known block to genesis block. It goes backward into blockchain like this: last ten blocks, then block in 2 blocks, block in 4 blocks, block in 8 blocks, ..., genesis block.

`first_block_timestamp` - timestamp

Optional.

bytecoind won't return blocks earlier than this point in time.

`max_count` - uint32

Optional.

Maximum number of blocks to return.

#### Request

```java
curl --location --request POST 'https://bcn.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "sync_blocks",
"params": {"sparse_chain": ["856c8547ad1864cc08664df8b41ebdd6ab8a01dd7ba04cbc06a428d64cd7e844"], "max_count": 2},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blocks": [
            {
                "header": {
                    "already_generated_coins": 18444651493828161509,
                    "already_generated_key_outputs": 63482012,
                    "already_generated_transactions": 6659692,
                    "base_reward": 7982589202,
                    "binary_nonce": "28c76209",
                    "block_capacity_vote": 100000,
                    "block_capacity_vote_median": 100000,
                    "block_size": 5707,
                    "cumulative_difficulty": 30627729797502340,
                    "difficulty": 11933350361,
                    "effective_size_median": 0,
                    "hash": "0012daca5694fd5f228e61144160a0f2c2b9920e467012930b3a67393c607988",
                    "height": 2398925,
                    "major_version": 4,
                    "minor_version": 7,
                    "nonce": 157468456,
                    "previous_block_hash": "856c8547ad1864cc08664df8b41ebdd6ab8a01dd7ba04cbc06a428d64cd7e844",
                    "reward": 7983279202,
                    "size_median": 0,
                    "timestamp": 1631266587,
                    "timestamp_median": 1631263229,
                    "transactions_fee": 690000,
                    "transactions_size": 5092
                },
                "output_stack_indexes": [
                    [
                        1472,
                        964,
                        791404,
                        801323,
                        1424618,
                        854652
                    ],
                    [
                        1652,
                        1204465,
                        774975
                    ]
                ],
                "raw_header": {
                    "coinbase_transaction": {
                        "extra": "01db4b1131bc6fb9c96e7f84291aff6947663887e5ea1042a3d11fac9592b2839a0403a08d06",
                        "inputs": [
                            {
                                "height": 2398925,
                                "type": "coinbase"
                            }
                        ],
                        "outputs": [
                            {
                                "amount": 279000,
                                "public_key": "09e5e9735184180875736ccf29f61b579b34accb79a86bd87a4135412122cf3c",
                                "type": "key"
                            },
                            {
                                "amount": 202,
                                "public_key": "57c2ff72ebbcf3e7a423ac9cc7febeee766e1c7e4870758ee431269d3cab5867",
                                "type": "key"
                            },
                            {
                                "amount": 3000000,
                                "public_key": "bfd0091cd7ce95536891b83aca07483d41cdad0a5aa18bae39676daac544f1ed",
                                "type": "key"
                            },
                            {
                                "amount": 80000000,
                                "public_key": "7c3d28c0b151bb812f9d198cb938d492887c9a2b5dbee18da36b46ab422d5681",
                                "type": "key"
                            },
                            {
                                "amount": 900000000,
                                "public_key": "af8c54a3d39abca4f09cf7556fc2d5b6ad5183e60abcdf58a6db1516c86b95a2",
                                "type": "key"
                            },
                            {
                                "amount": 7000000000,
                                "public_key": "8e8450f60036c828f83d8a2f70800e2029a7e982b4564c608e94f7e184ac59db",
                                "type": "key"
                            }
                        ],
                        "unlock_block_or_timestamp": 0,
                        "version": 1
                    },
                    "major_version": 4,
                    "minor_version": 7,
                    "previous_block_hash": "856c8547ad1864cc08664df8b41ebdd6ab8a01dd7ba04cbc06a428d64cd7e844",
                    "root_block": {
                        "blockchain_branch": [],
                        "coinbase_transaction": {
                            "extra": "019d5eeb8985fc4821ef9cf1dd0ede73cf1b2839ec446b13778185eb59bcf4a458021100000000338641b2176d8855ad000000000321006034fa13f2d74a9daed316da63fd2707f96a72f02e659e233e91e383d4d5b610",
                            "inputs": [
                                {
                                    "height": 660469,
                                    "type": "coinbase"
                                }
                            ],
                            "outputs": [
                                {
                                    "amount": 2,
                                    "public_key": "b60ecae662a57ee4f5cc2c14db0535c290013da05dbc24fc4988ecdef9b33478",
                                    "type": "key"
                                },
                                {
                                    "amount": 20,
                                    "public_key": "5430c49adbf7ffa75788ba20acf126e11104b05c4cc22eb6bf9b9ac792c25c62",
                                    "type": "key"
                                },
                                {
                                    "amount": 600,
                                    "public_key": "88924931480957967a1a5c611705e6e53cdfabf013e6d9c6d4ea871f708851e5",
                                    "type": "key"
                                },
                                {
                                    "amount": 3000,
                                    "public_key": "cffe33d4fdb85e2d4da16624134190acbc1c9f4cc7044d589300d0e128c2e340",
                                    "type": "key"
                                },
                                {
                                    "amount": 50000,
                                    "public_key": "31014cf71887788c50fa2037f332a299f4b92bf26b67c6643e751d23fadcd5b5",
                                    "type": "key"
                                },
                                {
                                    "amount": 800000,
                                    "public_key": "6355505712ef1140a317c158af0b9a3eb174761ba7d37c7e57f74a664ddf3d01",
                                    "type": "key"
                                },
                                {
                                    "amount": 9000000,
                                    "public_key": "76ad02f95e2ac21dfa351a0429c688dbec740a046fa891a64aef2d6d90b05d1e",
                                    "type": "key"
                                },
                                {
                                    "amount": 60000000,
                                    "public_key": "b7d0b37e0114ade840023ad754e6e01791eb948710dc4da6245b575aa0cef15a",
                                    "type": "key"
                                },
                                {
                                    "amount": 700000000,
                                    "public_key": "9f01a58c670ffd991ddc36d3d00c0d3ddf63a3fdc34b5cbb8beecc013b655035",
                                    "type": "key"
                                },
                                {
                                    "amount": 1000000000,
                                    "public_key": "ff0efdb3c4d5c717f8824de62e5d3e562eb81ec84fb15f4a6d32c75b6f4f85ef",
                                    "type": "key"
                                },
                                {
                                    "amount": 70000000000,
                                    "public_key": "b40d1a2bc28b450dc67e7e053181e6d26963a58559723e5f8fe672b0a60cd592",
                                    "type": "key"
                                },
                                {
                                    "amount": 3000000000000,
                                    "public_key": "1445ecf2dbb357499fc967ef00393ec82ad04d280bfc33115031ce55c13afc5d",
                                    "type": "key"
                                }
                            ],
                            "unlock_block_or_timestamp": 660479,
                            "version": 1
                        },
                        "coinbase_transaction_branch": [],
                        "major_version": 4,
                        "minor_version": 0,
                        "nonce": "28c76209",
                        "previous_block_hash": "8ac5d16188dfde7c208ab8c64c58c6f64dd2befec00e498920f29f30296c1767",
                        "timestamp": 1631266587,
                        "transaction_count": 1
                    },
                    "transaction_hashes": [
                        "11162425fffbfca999af3227c102431a52683ee31bb770e517d663ada4b1c9de"
                    ]
                },
                "raw_transactions": [
                    {
                        "extra": "",
                        "inputs": [
                            {
                                "amount": 100000,
                                "key_image": "10ae35a6a004323d44640ec89322ef7149e13d6bd57d34111230c4b6883eb18e",
                                "output_indexes": [
                                    20683,
                                    430,
                                    395,
                                    225
                                ],
                                "type": "key"
                            },
                            {
                                "amount": 955000,
                                "key_image": "750fa05f975ee72d79e8708ce917e2e1bbc2014762a7d9ce93687e4972dea386",
                                "output_indexes": [
                                    1033,
                                    45,
                                    57,
                                    444
                                ],
                                "type": "key"
                            },
                            {
                                "amount": 964000,
                                "key_image": "5fc89c0897143f55c8fa4f181c5e4f5263b7d0ca6abca0edd6a04dd3ce6024c8",
                                "output_indexes": [
                                    134,
                                    299,
                                    941,
                                    177
                                ],
                                "type": "key"
                            },
                            {
                                "amount": 964000,
                                "key_image": "f7fcc0f7f6e7adeed21ba084d3d50a8368534e94fd7ddd7d6c835db4901f5098",
                                "output_indexes": [
                                    1310,
                                    231,
                                    9,
                                    7
                                ],
                                "type": "key"
                            },
                            {
                                "amount": 1000000,
                                "key_image": "69c9d77902091eb6512495aa2bfdd95ce8373c2aa56269e7a9b1cd394bb6ff46",
                                "output_indexes": [
                                    789246,
                                    7841,
                                    79,
                                    20
                                ],
                                "type": "key"
                            },
                            {
                                "amount": 400000000,
                                "key_image": "f5f74c3d1d7bee1adca019dacb89ad44e9d30ead696168c75d79acaf321610d3",
                                "output_indexes": [
                                    850671,
                                    2184,
                                    629,
                                    358
                                ],
                                "type": "key"
                            },
                            {
                                "amount": 500000000,
                                "key_image": "e0da440b5862e8545c204e5deaefb8f11c8c9ed8c1741bf7638fb16bf86800b9",
                                "output_indexes": [
                                    921339,
                                    1835,
                                    113,
                                    188
                                ],
                                "type": "key"
                            }
                        ],
                        "outputs": [
                            {
                                "amount": 63000,
                                "encrypted_address_type": "60",
                                "encrypted_secret": "e257b927ab7a3c59589b22cd6f951155446f540e522f7878151f1e57bb7da35b",
                                "public_key": "7fe1fc21c1f73822203164bc58912cb3e0b1f3b33735654837e74299fa214e8e",
                                "type": "key"
                            },
                            {
                                "amount": 1000000000,
                                "encrypted_address_type": "53",
                                "encrypted_secret": "b1418006cd547a447f64cd36a08fc7fbbfedbeb840849cb4d67b9677af620848",
                                "public_key": "fcc4840d354fe3976f83ef441817145d40e8befd0bbffc7d1fbaf79b37ec3354",
                                "type": "key"
                            },
                            {
                                "amount": 70000000000,
                                "encrypted_address_type": "cd",
                                "encrypted_secret": "aa831783edc04ab91f2e7333097c19ef482b39196f09f058c528ab08c32a1707",
                                "public_key": "1357c429ec9857290b4fef8188fcdf20835434b3b0a8db968a5e3de238c8031c",
                                "type": "key"
                            }
                        ],
                        "unlock_block_or_timestamp": 0,
                        "version": 4
                    }
                ],
                "transactions": [
                    {
                        "amount": 7983279202,
                        "anonymity": 0,
                        "binary_size": 269,
                        "block_hash": "0012daca5694fd5f228e61144160a0f2c2b9920e467012930b3a67393c607988",
                        "block_height": 2398925,
                        "coinbase": true,
                        "extra": "01db4b1131bc6fb9c96e7f84291aff6947663887e5ea1042a3d11fac9592b2839a0403a08d06",
                        "fee": 0,
                        "hash": "89eb3525d61f10dc868e0b23151b27aee14808c75417dc43dff7d22aed1f7f16",
                        "inputs_hash": "6ec2c9006d228c9aaf5d11798e74159f7612341c9b9a18fe8adf85b88559d80c",
                        "prefix_hash": "89eb3525d61f10dc868e0b23151b27aee14808c75417dc43dff7d22aed1f7f16",
                        "public_key": "db4b1131bc6fb9c96e7f84291aff6947663887e5ea1042a3d11fac9592b2839a",
                        "size": 269,
                        "timestamp": 1631266587,
                        "unlock_block_or_timestamp": 0,
                        "unlock_time": 0
                    },
                    {
                        "amount": 71000063000,
                        "anonymity": 3,
                        "binary_size": 4823,
                        "block_hash": "0012daca5694fd5f228e61144160a0f2c2b9920e467012930b3a67393c607988",
                        "block_height": 2398925,
                        "coinbase": false,
                        "fee": 690000,
                        "hash": "11162425fffbfca999af3227c102431a52683ee31bb770e517d663ada4b1c9de",
                        "inputs_hash": "272c3b5045236abee87bd12ad6127239c8ad01e2680b23e36472346b56c86dbb",
                        "prefix_hash": "c89c3c639723ccd0bc3d12b4457af2519931b8e607682b0f7ed50e7a7b888253",
                        "size": 4823,
                        "timestamp": 1631266587,
                        "unlock_block_or_timestamp": 0,
                        "unlock_time": 0
                    }
                ]
            },
            {
                "header": {
                    "already_generated_coins": 18444651501810720260,
                    "already_generated_key_outputs": 63482018,
                    "already_generated_transactions": 6659693,
                    "base_reward": 7982558751,
                    "binary_nonce": "9041a6f3",
                    "block_capacity_vote": 100000,
                    "block_capacity_vote_median": 100000,
                    "block_size": 527,
                    "cumulative_difficulty": 30627741738754870,
                    "difficulty": 11941252530,
                    "effective_size_median": 0,
                    "hash": "507fadea86c1c5113d77adc497e544e236b6ea081e214792f212a06ffa743869",
                    "height": 2398926,
                    "major_version": 4,
                    "minor_version": 7,
                    "nonce": 4087759248,
                    "previous_block_hash": "0012daca5694fd5f228e61144160a0f2c2b9920e467012930b3a67393c607988",
                    "reward": 7982558751,
                    "size_median": 0,
                    "timestamp": 1631266889,
                    "timestamp_median": 1631263236,
                    "transactions_fee": 0,
                    "transactions_size": 268
                },
                "output_stack_indexes": [
                    [
                        1744,
                        954,
                        833938,
                        801324,
                        1424619,
                        854653
                    ]
                ],
                "raw_header": {
                    "coinbase_transaction": {
                        "extra": "012a5c6d803d1b2573f28a81e4706c5d70b787414ac43106696e35a97ac1f391160403a08d06",
                        "inputs": [
                            {
                                "height": 2398926,
                                "type": "coinbase"
                            }
                        ],
                        "outputs": [
                            {
                                "amount": 558000,
                                "public_key": "699b978a1486238d32c1c6bb562df525e39cac44d2d17533814c9737d753a834",
                                "type": "key"
                            },
                            {
                                "amount": 751,
                                "public_key": "7ee00c174ef902209e011ccb21b78eb5a4370b6ce3402a99969c7a196bf53148",
                                "type": "key"
                            },
                            {
                                "amount": 2000000,
                                "public_key": "7baf0b59991bb4e4fe9eea9035bb6df872a9c7bb367e2d92e84518d568ac677b",
                                "type": "key"
                            },
                            {
                                "amount": 80000000,
                                "public_key": "89f86afd958a21e4d5c0cd7f70167682fcbfb85ca76d239618c4e54bc1a42bfd",
                                "type": "key"
                            },
                            {
                                "amount": 900000000,
                                "public_key": "37205f4badcfc037f552aab67f5d29cb388918711d5bbacdeda71f454b6e26c3",
                                "type": "key"
                            },
                            {
                                "amount": 7000000000,
                                "public_key": "96ca9a86e8ef567d4edc25a72561b9c5573b7c67365758c60ffb1070ac436663",
                                "type": "key"
                            }
                        ],
                        "unlock_block_or_timestamp": 0,
                        "version": 1
                    },
                    "major_version": 4,
                    "minor_version": 7,
                    "previous_block_hash": "0012daca5694fd5f228e61144160a0f2c2b9920e467012930b3a67393c607988",
                    "root_block": {
                        "blockchain_branch": [],
                        "coinbase_transaction": {
                            "extra": "017727d7f42013bf653a2cbd97c385785eaa37da01c73a50e20a63a14f7e2f832a0211000000a398d9879272d544620000000000032100ff3bb1e5d57d44562ab4e6d7f7a693a40d1949b1f7b89c81fa3ef3ad5eb9be9c",
                            "ignored": 0,
                            "inputs": [
                                {
                                    "height": 2454172,
                                    "type": "coinbase"
                                }
                            ],
                            "outputs": [
                                {
                                    "amount": 3000000000000,
                                    "public_key": "3bb5e0fa2d6e0bf116558fbb35d1c68c95ef423ee0191a6bfc42b2550a79b5f0",
                                    "type": "key"
                                },
                                {
                                    "amount": 7000000000000,
                                    "public_key": "a8f5c7211fdd68490784d0013958520adf4e3e088f299671c137bb367923b411",
                                    "type": "key"
                                }
                            ],
                            "unlock_block_or_timestamp": 2454232,
                            "version": 2
                        },
                        "coinbase_transaction_branch": [],
                        "major_version": 60,
                        "minor_version": 60,
                        "nonce": "9041a6f3",
                        "previous_block_hash": "5a95458ebaa3de7697b543a6b6809ad94ea988c4b4fe6112523da367358d8cba",
                        "timestamp": 1631266889,
                        "transaction_count": 1
                    },
                    "transaction_hashes": []
                },
                "raw_transactions": [],
                "transactions": [
                    {
                        "amount": 7982558751,
                        "anonymity": 0,
                        "binary_size": 268,
                        "block_hash": "507fadea86c1c5113d77adc497e544e236b6ea081e214792f212a06ffa743869",
                        "block_height": 2398926,
                        "coinbase": true,
                        "extra": "012a5c6d803d1b2573f28a81e4706c5d70b787414ac43106696e35a97ac1f391160403a08d06",
                        "fee": 0,
                        "hash": "8c3527b19b717cdfa52450dfd09e3770aec549929c98d9767ea5b2ea5b2b2b99",
                        "inputs_hash": "c04073a6ea5b7c6a7c22e20a5ffbb2cd1df82f1344678723ab18b68e1214d78a",
                        "prefix_hash": "8c3527b19b717cdfa52450dfd09e3770aec549929c98d9767ea5b2ea5b2b2b99",
                        "public_key": "2a5c6d803d1b2573f28a81e4706c5d70b787414ac43106696e35a97ac1f39116",
                        "size": 268,
                        "timestamp": 1631266889,
                        "unlock_block_or_timestamp": 0,
                        "unlock_time": 0
                    }
                ]
            }
        ],
        "start_height": 2398925,
        "status": {
            "incoming_peer_count": 0,
            "lower_level_error": "",
            "next_block_effective_median_size": 98872,
            "outgoing_peer_count": 8,
            "recommended_fee_per_byte": 100,
            "recommended_max_transaction_size": 98872,
            "top_block_cumulative_difficulty": 30676358330795090,
            "top_block_difficulty": 10361588328,
            "top_block_hash": "5099fb0b04b83d4f9862109608349d7a25938e6aa00b509366848589fe69917a",
            "top_block_height": 2403323,
            "top_block_timestamp": 1631799662,
            "top_block_timestamp_median": 1631795442,
            "top_known_block_height": 2403323,
            "transaction_pool_version": 2438
        }
    }
}
```
