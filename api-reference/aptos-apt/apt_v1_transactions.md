---
description: >-
  Example code for the /v1/transactions json-rpc method. Сomplete guide on how
  to use /v1/transactions json-rpc in GetBlock.io Web3 documentation.
---

# /v1/transactions - Aptos

#### Parameters

`limit` -

Maximum number of transactions to retrieve. Gets default page size if not provided.

`start` -

Optional starting sequence number of events. Defaults to the latest transactions.

#### Request

```java
curl --location --request GET 'https://apt.getblock.io/v1/transactions?limit=3' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
```

#### Response

```java
[
    {
        "accumulator_root_hash": "0xdf24383f1795b508008f080f598d7b0b3be5bf49e760ded6da8c88d86c33ec9d",
        "changes": [
            {
                "address": "0x54ad3d30af77b60d939ae356e6606de9a4da67583f02b962d2d3f2e481484e90",
                "data": {
                    "data": {
                        "oracle_events": {
                            "counter": "1064682",
                            "guid": {
                                "id": {
                                    "addr": "0x54ad3d30af77b60d939ae356e6606de9a4da67583f02b962d2d3f2e481484e90",
                                    "creation_num": "15"
                                }
                            }
                        },
                        "relayer_events": {
                            "counter": "1064670",
                            "guid": {
                                "id": {
                                    "addr": "0x54ad3d30af77b60d939ae356e6606de9a4da67583f02b962d2d3f2e481484e90",
                                    "creation_num": "16"
                                }
                            }
                        }
                    },
                    "type": "0x54ad3d30af77b60d939ae356e6606de9a4da67583f02b962d2d3f2e481484e90::uln_receive::EventStore"
                },
                "state_key_hash": "0x7d72ed827ba7631767ca4d5b692e052bdfdc02b962acd9aa702639b297d5d05a",
                "type": "write_resource"
            },
            {
                "address": "0xc20ea5a196c81d8d7aff814aa37f8a5823acffbc4193efd3b2aafc9ef2803255",
                "data": {
                    "data": {
                        "coin": {
                            "value": "42470516255"
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
                },
                "state_key_hash": "0xb2fcff321d24337870b8afa2197359fcb527d2529e51398a0b7165649f2a0801",
                "type": "write_resource"
            },
            {
                "address": "0xc20ea5a196c81d8d7aff814aa37f8a5823acffbc4193efd3b2aafc9ef2803255",
                "data": {
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
                        "sequence_number": "497661",
                        "signer_capability_offer": {
                            "for": {
                                "vec": []
                            }
                        }
                    },
                    "type": "0x1::account::Account"
                },
                "state_key_hash": "0x0d90a983bdbbfccd6140bc87ccaaf610d4075289145ce60a4cf17f3223574ecd",
                "type": "write_resource"
            },
            {
                "data": null,
                "handle": "0x1b854694ae746cdbd8d44186ca4929b2b337df21d1c74633be19b2710552fdca",
                "key": "0x0619dc29a0aac8fa146714058e8dd6d2d0f3bdf5f6331907bf91f3acd81e6935",
                "state_key_hash": "0x6e4b28d40f98a106a65163530924c0dcb40c1349d3aa915d108b4d6cfc1ddb19",
                "type": "write_table_item",
                "value": "0x47f6d099db9770010000000000000000"
            },
            {
                "data": null,
                "handle": "0xcc6c608b81fcf6d6cc0b3e9c07efb69f897bf80605e8279fc7e932fa7c12130d",
                "key": "0x2042678e0b5a3409e6cb67ddb626e61b3cbc09c65bd8636c3a92554446e883a859",
                "state_key_hash": "0x2574694216b5a51a74960797f1d3c1680ae5c0582a9217c96bfe116c1396aff3",
                "type": "write_table_item",
                "value": "0x01"
            },
            {
                "data": null,
                "handle": "0xdcd5cc59d7b3d2b975bf7fcd73faa1c76ac5d3ec7479c61316af3a03ea5db3f5",
                "key": "0x12e12de0af996d9611b0b78928cd9f4cbf50d94d972043cdd829baa77a78929b200c8a3237d64bf11673b2d1ef6e7d0e013e1d61ece6e606c7d342a8493b78baff",
                "state_key_hash": "0xc726f12cfcb45b2f9561685cb3430eb3bcef7a360e59b27de351efae689a0347",
                "type": "write_table_item",
                "value": "0x1400000000000000"
            }
        ],
        "event_root_hash": "0x259f6232cd763fa77900080ee376b20862aa7b6416e035a9ea813b96e41d5626",
        "events": [
            {
                "data": {
                    "confirmations": "20",
                    "hash": "0x0c8a3237d64bf11673b2d1ef6e7d0e013e1d61ece6e606c7d342a8493b78baff",
                    "signer": "0x12e12de0af996d9611b0b78928cd9f4cbf50d94d972043cdd829baa77a78929b"
                },
                "guid": {
                    "account_address": "0x54ad3d30af77b60d939ae356e6606de9a4da67583f02b962d2d3f2e481484e90",
                    "creation_number": "15"
                },
                "sequence_number": "1064681",
                "type": "0x54ad3d30af77b60d939ae356e6606de9a4da67583f02b962d2d3f2e481484e90::uln_receive::SignerEvent"
            }
        ],
        "expiration_timestamp_secs": "1685697287",
        "gas_unit_price": "120",
        "gas_used": "939",
        "hash": "0x6041a16d053ed12cb5dd185d4443d1ee79a302bfa2a05b2756156ca43140b78f",
        "max_gas_amount": "200000",
        "payload": {
            "arguments": [
                "0x0c8a3237d64bf11673b2d1ef6e7d0e013e1d61ece6e606c7d342a8493b78baff",
                "20",
                "1685717681",
                "0x945c919bc348e094057f7a9f17dcb5ad17b87c0dc079120c9910d0ad68bd55274d3016421f3521fcb2896dcff7cdfc1e505e52910f6c6a37a97b9ef17d3ec5c9011510a8e2fa099c25b0917b40ca88eaccf3fec37c91d8df43afe0e7733a600c0b6bf90a2a277a0e228188bac85659bec32be770ccb6c2c2db2bb765cd63a367be00"
            ],
            "function": "0xc2846ea05319c339b3b52186ceae40b43d4e9cf6c7350336c3eb0b351d9394eb::oracle::mso_propose",
            "type": "entry_function_payload",
            "type_arguments": []
        },
        "sender": "0xc20ea5a196c81d8d7aff814aa37f8a5823acffbc4193efd3b2aafc9ef2803255",
        "sequence_number": "497660",
        "signature": {
            "public_key": "0xf22c5aeb53fc98ec8196d88df86df952a616108eabdbb993f14805660a8b1831",
            "signature": "0x605835fdb308777a70c7cff179562dd7d0857cfd3b44395eb41741b99ad839064b9b449b3e88bb4084a2a7c21b2d1e245c8099199b6b7a8d19c5de688105a301",
            "type": "ed25519_signature"
        },
        "state_change_hash": "0x157737081d9b412f3cb6d90329877187445c73a948892c1289545f5f0d065537",
        "state_checkpoint_hash": null,
        "success": true,
        "timestamp": "1685696086904097",
        "type": "user_transaction",
        "version": "152087596",
        "vm_status": "Executed successfully"
    },
    {
        "accumulator_root_hash": "0xc74dca60ff612c6df1e3b208efd04091076e94290e9095c9fb4d458729ec6191",
        "changes": [
            {
                "address": "0x235d28281f2e0508fb2cba59e06b0f33d120473d7103548df417f351e191cba5",
                "data": {
                    "data": {
                        "coin": {
                            "value": "30309000"
                        },
                        "deposit_events": {
                            "counter": "1",
                            "guid": {
                                "id": {
                                    "addr": "0x235d28281f2e0508fb2cba59e06b0f33d120473d7103548df417f351e191cba5",
                                    "creation_num": "2"
                                }
                            }
                        },
                        "frozen": false,
                        "withdraw_events": {
                            "counter": "0",
                            "guid": {
                                "id": {
                                    "addr": "0x235d28281f2e0508fb2cba59e06b0f33d120473d7103548df417f351e191cba5",
                                    "creation_num": "3"
                                }
                            }
                        }
                    },
                    "type": "0x1::coin::CoinStore<0x1::aptos_coin::AptosCoin>"
                },
                "state_key_hash": "0xf42cf3525744edad5cc7033151f77da83032c9a32ab6a70fd33de047567f8c5a",
                "type": "write_resource"
            },
            {
                "address": "0x235d28281f2e0508fb2cba59e06b0f33d120473d7103548df417f351e191cba5",
                "data": {
                    "data": {
                        "coin": {
                            "value": "0"
                        },
                        "deposit_events": {
                            "counter": "0",
                            "guid": {
                                "id": {
                                    "addr": "0x235d28281f2e0508fb2cba59e06b0f33d120473d7103548df417f351e191cba5",
                                    "creation_num": "4"
                                }
                            }
                        },
                        "frozen": false,
                        "withdraw_events": {
                            "counter": "0",
                            "guid": {
                                "id": {
                                    "addr": "0x235d28281f2e0508fb2cba59e06b0f33d120473d7103548df417f351e191cba5",
                                    "creation_num": "5"
                                }
                            }
                        }
                    },
                    "type": "0x1::coin::CoinStore<0xf22bede237a07e121b56d91a491eb7bcdfd1f5907926a9e58338f964a01b17fa::asset::USDT>"
                },
                "state_key_hash": "0xb1b47826bbc7a3a8926e0c0a5bbe0a8879f68d3738901d763c92ed8a3e274d76",
                "type": "write_resource"
            },
            {
                "address": "0x235d28281f2e0508fb2cba59e06b0f33d120473d7103548df417f351e191cba5",
                "data": {
                    "data": {
                        "authentication_key": "0x235d28281f2e0508fb2cba59e06b0f33d120473d7103548df417f351e191cba5",
                        "coin_register_events": {
                            "counter": "2",
                            "guid": {
                                "id": {
                                    "addr": "0x235d28281f2e0508fb2cba59e06b0f33d120473d7103548df417f351e191cba5",
                                    "creation_num": "0"
                                }
                            }
                        },
                        "guid_creation_num": "6",
                        "key_rotation_events": {
                            "counter": "0",
                            "guid": {
                                "id": {
                                    "addr": "0x235d28281f2e0508fb2cba59e06b0f33d120473d7103548df417f351e191cba5",
                                    "creation_num": "1"
                                }
                            }
                        },
                        "rotation_capability_offer": {
                            "for": {
                                "vec": []
                            }
                        },
                        "sequence_number": "1",
                        "signer_capability_offer": {
                            "for": {
                                "vec": []
                            }
                        }
                    },
                    "type": "0x1::account::Account"
                },
                "state_key_hash": "0x5091560eb66eeceada158e6d22b3fb9bbaee41c8f597d7e804b04b2858e30634",
                "type": "write_resource"
            },
            {
                "data": null,
                "handle": "0x1b854694ae746cdbd8d44186ca4929b2b337df21d1c74633be19b2710552fdca",
                "key": "0x0619dc29a0aac8fa146714058e8dd6d2d0f3bdf5f6331907bf91f3acd81e6935",
                "state_key_hash": "0x6e4b28d40f98a106a65163530924c0dcb40c1349d3aa915d108b4d6cfc1ddb19",
                "type": "write_table_item",
                "value": "0x0f2fd099db9770010000000000000000"
            }
        ],
        "event_root_hash": "0xa9193bf47810af0e11dbd1021605d6fdcda9efc3f642d82b9b7223c5bcfec157",
        "events": [
            {
                "data": {
                    "type_info": {
                        "account_address": "0xf22bede237a07e121b56d91a491eb7bcdfd1f5907926a9e58338f964a01b17fa",
                        "module_name": "0x6173736574",
                        "struct_name": "0x55534454"
                    }
                },
                "guid": {
                    "account_address": "0x235d28281f2e0508fb2cba59e06b0f33d120473d7103548df417f351e191cba5",
                    "creation_number": "0"
                },
                "sequence_number": "1",
                "type": "0x1::account::CoinRegisterEvent"
            }
        ],
        "expiration_timestamp_secs": "1685696094",
        "gas_unit_price": "100",
        "gas_used": "510",
        "hash": "0x70db890ab89190f01f537f7096b825ae20237c2d795e7cf44f60688b751867cf",
        "max_gas_amount": "686",
        "payload": {
            "arguments": [],
            "function": "0x1::managed_coin::register",
            "type": "entry_function_payload",
            "type_arguments": [
                "0xf22bede237a07e121b56d91a491eb7bcdfd1f5907926a9e58338f964a01b17fa::asset::USDT"
            ]
        },
        "sender": "0x235d28281f2e0508fb2cba59e06b0f33d120473d7103548df417f351e191cba5",
        "sequence_number": "0",
        "signature": {
            "public_key": "0x9c433e6d0e5da36ec81e03a2468a090fcf7ae942a5f12416c9d21bd1039c93fd",
            "signature": "0x3dfdf909b8d1c0c7ca8e61bda8fead489977fd3be77744ce0e76022fc2d7e201514172b366105077d50af124a69bda1a9f8d52542e72ad68dcf4dc3ef41a4a0a",
            "type": "ed25519_signature"
        },
        "state_change_hash": "0xc4ade06ffe7f6945724ec75a1a2737c67139fa33051b61a359683d16099a10c3",
        "state_checkpoint_hash": null,
        "success": true,
        "timestamp": "1685696086904097",
        "type": "user_transaction",
        "version": "152087597",
        "vm_status": "Executed successfully"
    },
    {
        "accumulator_root_hash": "0xd3c3ad85e2d6bca20bd5644811216c29721a99a07c6677b2d0e12c9d313083a3",
        "changes": [],
        "event_root_hash": "0x414343554d554c41544f525f504c414345484f4c4445525f4841534800000000",
        "gas_used": "0",
        "hash": "0x2d6fa8e84d09299cdfba10823cee0bed24b8c9f9a6f678db450e3fd99a2aef7f",
        "state_change_hash": "0xafb6e14fe47d850fd0a7395bcfb997ffacf4715e0f895cc162c218e4a7564bc6",
        "state_checkpoint_hash": "0x03be61d0ff5fed1c0ed33d8d8372c94c1dc8dd5b848e806ab5aa36fcb4d49cdb",
        "success": true,
        "timestamp": "1685696086904097",
        "type": "state_checkpoint_transaction",
        "version": "152087598",
        "vm_status": "Executed successfully"
    }
]
```
