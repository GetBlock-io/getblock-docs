---
description: >-
  Example code for the /v1/blocks/by_version/{version} json-rpc method. Сomplete
  guide on how to use /v1/blocks/by_version/{version} json-rpc in GetBlock.io
  Web3 documentation.
---

# /v1/blocks/by\_version/{version} - Aptos

#### Parameters

`with_transactions` -

Includes transactions in the block if true. Defaults to false.

#### Request

```java
curl --location --request GET 'https://apt.getblock.io/v1/blocks/by_version/147811395?with_transactions=true' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
```

#### Response

```java
{
    "block_hash": "0x3a7b9293154d52b090a3ee601cc4add1efd82668613b1fd103197109ff54be2c",
    "block_height": "57347454",
    "block_timestamp": "1685063032234522",
    "first_version": "147811395",
    "last_version": "147811397",
    "transactions": [
        {
            "accumulator_root_hash": "0xea623df05c583207535aacb8a5cdc24562e10733fcfd1dbc0befc255d58a7d43",
            "changes": [
                {
                    "address": "0x1",
                    "data": {
                        "data": {
                            "epoch_interval": "7200000000",
                            "height": "57347454",
                            "new_block_events": {
                                "counter": "57347455",
                                "guid": {
                                    "id": {
                                        "addr": "0x1",
                                        "creation_num": "3"
                                    }
                                }
                            },
                            "update_epoch_interval_events": {
                                "counter": "0",
                                "guid": {
                                    "id": {
                                        "addr": "0x1",
                                        "creation_num": "4"
                                    }
                                }
                            }
                        },
                        "type": "0x1::block::BlockResource"
                    },
                    "state_key_hash": "0x5ddf404c60e96e9485beafcabb95609fed8e38e941a725cae4dcec8296fb32d7",
                    "type": "write_resource"
                },
                {
                    "address": "0x1",
                    "data": {
                        "data": {
                            "validators": [
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "8"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "16"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "5"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "8"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "9"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "63"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "31"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "14"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "4"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "29"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "43"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "41"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "39"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "52"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "107"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "5"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "10"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "9"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "15"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "11"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "193"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "196"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "182"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "217"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "193"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "206"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "206"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "210"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "172"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "206"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "139"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "12"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "5"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "14"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "11"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "10"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "12"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "11"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "11"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "17"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "5"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "6"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "11"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "205"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "15"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "179"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "18"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "8"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "15"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "9"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "159"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "189"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "6"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "15"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "43"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "11"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "135"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "16"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "15"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "8"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "10"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "11"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "191"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "200"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "142"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "142"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "15"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "13"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "89"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "11"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "16"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "7"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "4"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "42"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "47"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "30"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "16"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "212"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "194"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "9"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "179"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "9"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "9"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "192"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "11"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "194"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "60"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "48"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "196"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "142"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "145"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "9"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "161"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "207"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "211"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "147"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "159"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "21"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "141"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "17"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "7"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "4"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "11"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "11"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "107"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "10"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "128"
                                },
                                {
                                    "failed_proposals": "0",
                                    "successful_proposals": "72"
                                }
                            ]
                        },
                        "type": "0x1::stake::ValidatorPerformance"
                    },
                    "state_key_hash": "0x8048c954221814b04533a9f0a9946c3a8d472ac62df5accb9f47c097e256e8b6",
                    "type": "write_resource"
                },
                {
                    "address": "0x1",
                    "data": {
                        "data": {
                            "microseconds": "1685063032234522"
                        },
                        "type": "0x1::timestamp::CurrentTimeMicroseconds"
                    },
                    "state_key_hash": "0x7b1615bf012d3c94223f3f76287ee2f7bdf31d364071128b256aeff0841b626d",
                    "type": "write_resource"
                }
            ],
            "epoch": "2708",
            "event_root_hash": "0x021fff75f2338698ff461969deb5b907aa617c6f83e988c3473e5f3f77debb84",
            "events": [
                {
                    "data": {
                        "epoch": "2708",
                        "failed_proposer_indices": [],
                        "hash": "0x3a7b9293154d52b090a3ee601cc4add1efd82668613b1fd103197109ff54be2c",
                        "height": "57347454",
                        "previous_block_votes_bitvec": "0x7ffe8dca8332f836c96afaff8e90",
                        "proposer": "0x5ae6127c854988607bb3375484e61ba10e2ce5e6ca9ff92a7b322149ddbbf552",
                        "round": "7819",
                        "time_microseconds": "1685063032234522"
                    },
                    "guid": {
                        "account_address": "0x1",
                        "creation_number": "3"
                    },
                    "sequence_number": "57347454",
                    "type": "0x1::block::NewBlockEvent"
                }
            ],
            "failed_proposer_indices": [],
            "gas_used": "0",
            "hash": "0xa1e9ce115bbfb5e3ff06df3beb3c2849cb8d48fb6474d995e0b94af1d0c3dc4f",
            "id": "0x3a7b9293154d52b090a3ee601cc4add1efd82668613b1fd103197109ff54be2c",
            "previous_block_votes_bitvec": [
                127,
                254,
                141,
                202,
                131,
                50,
                248,
                54,
                201,
                106,
                250,
                255,
                142,
                144
            ],
            "proposer": "0x5ae6127c854988607bb3375484e61ba10e2ce5e6ca9ff92a7b322149ddbbf552",
            "round": "7819",
            "state_change_hash": "0xfc49459fab7265e5f7e20c24eb6abe5a68eeadf39b1f7811d62dece88e71ad0d",
            "state_checkpoint_hash": null,
            "success": true,
            "timestamp": "1685063032234522",
            "type": "block_metadata_transaction",
            "version": "147811395",
            "vm_status": "Executed successfully"
        },
        {
            "accumulator_root_hash": "0x966dd8c623b3063a58c46bf64fbd95d06746779aa44188fd7dadd4602b25acaf",
            "changes": [
                {
                    "address": "0x4c1b416e4e3793724dabd2d10909689bd534159a2d4d4b605ec22d9e593bbad3",
                    "data": {
                        "data": {
                            "coin": {
                                "value": "4948300"
                            },
                            "deposit_events": {
                                "counter": "1",
                                "guid": {
                                    "id": {
                                        "addr": "0x4c1b416e4e3793724dabd2d10909689bd534159a2d4d4b605ec22d9e593bbad3",
                                        "creation_num": "2"
                                    }
                                }
                            },
                            "frozen": false,
                            "withdraw_events": {
                                "counter": "0",
                                "guid": {
                                    "id": {
                                        "addr": "0x4c1b416e4e3793724dabd2d10909689bd534159a2d4d4b605ec22d9e593bbad3",
                                        "creation_num": "3"
                                    }
                                }
                            }
                        },
                        "type": "0x1::coin::CoinStore<0x1::aptos_coin::AptosCoin>"
                    },
                    "state_key_hash": "0x3cec7fe8530a6688d69c124ed977319e22d8a8a74acd6b95eaa568c7726e7a74",
                    "type": "write_resource"
                },
                {
                    "address": "0x4c1b416e4e3793724dabd2d10909689bd534159a2d4d4b605ec22d9e593bbad3",
                    "data": {
                        "data": {
                            "coin": {
                                "value": "50000"
                            },
                            "deposit_events": {
                                "counter": "1",
                                "guid": {
                                    "id": {
                                        "addr": "0x4c1b416e4e3793724dabd2d10909689bd534159a2d4d4b605ec22d9e593bbad3",
                                        "creation_num": "4"
                                    }
                                }
                            },
                            "frozen": false,
                            "withdraw_events": {
                                "counter": "0",
                                "guid": {
                                    "id": {
                                        "addr": "0x4c1b416e4e3793724dabd2d10909689bd534159a2d4d4b605ec22d9e593bbad3",
                                        "creation_num": "5"
                                    }
                                }
                            }
                        },
                        "type": "0x1::coin::CoinStore<0xf22bede237a07e121b56d91a491eb7bcdfd1f5907926a9e58338f964a01b17fa::asset::USDT>"
                    },
                    "state_key_hash": "0xb97e0a59827e06be4f337a74db01a6f044ca059fb389f702a140d00378fba6de",
                    "type": "write_resource"
                },
                {
                    "address": "0x4c1b416e4e3793724dabd2d10909689bd534159a2d4d4b605ec22d9e593bbad3",
                    "data": {
                        "data": {
                            "authentication_key": "0x4c1b416e4e3793724dabd2d10909689bd534159a2d4d4b605ec22d9e593bbad3",
                            "coin_register_events": {
                                "counter": "2",
                                "guid": {
                                    "id": {
                                        "addr": "0x4c1b416e4e3793724dabd2d10909689bd534159a2d4d4b605ec22d9e593bbad3",
                                        "creation_num": "0"
                                    }
                                }
                            },
                            "guid_creation_num": "6",
                            "key_rotation_events": {
                                "counter": "0",
                                "guid": {
                                    "id": {
                                        "addr": "0x4c1b416e4e3793724dabd2d10909689bd534159a2d4d4b605ec22d9e593bbad3",
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
                    "state_key_hash": "0x65a1189e97f8ec70eafc8ece99b25f79ed0e230295c5d7bf1f46b2fa55093491",
                    "type": "write_resource"
                },
                {
                    "address": "0xf22bede237a07e121b56d91a491eb7bcdfd1f5907926a9e58338f964a01b17fa",
                    "data": {
                        "data": {
                            "decimals": 6,
                            "name": "Tether USD",
                            "supply": {
                                "vec": [
                                    {
                                        "aggregator": {
                                            "vec": []
                                        },
                                        "integer": {
                                            "vec": [
                                                {
                                                    "limit": "340282366920938463463374607431768211455",
                                                    "value": "6940588922528"
                                                }
                                            ]
                                        }
                                    }
                                ]
                            },
                            "symbol": "USDT"
                        },
                        "type": "0x1::coin::CoinInfo<0xf22bede237a07e121b56d91a491eb7bcdfd1f5907926a9e58338f964a01b17fa::asset::USDT>"
                    },
                    "state_key_hash": "0x18a12ea319b7376eb20ec94a19057bd9295ed5f4c21719f759b01a0a7b53f546",
                    "type": "write_resource"
                },
                {
                    "address": "0xf22bede237a07e121b56d91a491eb7bcdfd1f5907926a9e58338f964a01b17fa",
                    "data": {
                        "data": {
                            "claim_events": {
                                "counter": "186073",
                                "guid": {
                                    "id": {
                                        "addr": "0xf22bede237a07e121b56d91a491eb7bcdfd1f5907926a9e58338f964a01b17fa",
                                        "creation_num": "6"
                                    }
                                }
                            },
                            "receive_events": {
                                "counter": "889351",
                                "guid": {
                                    "id": {
                                        "addr": "0xf22bede237a07e121b56d91a491eb7bcdfd1f5907926a9e58338f964a01b17fa",
                                        "creation_num": "5"
                                    }
                                }
                            },
                            "send_events": {
                                "counter": "622265",
                                "guid": {
                                    "id": {
                                        "addr": "0xf22bede237a07e121b56d91a491eb7bcdfd1f5907926a9e58338f964a01b17fa",
                                        "creation_num": "4"
                                    }
                                }
                            }
                        },
                        "type": "0xf22bede237a07e121b56d91a491eb7bcdfd1f5907926a9e58338f964a01b17fa::coin_bridge::EventStore"
                    },
                    "state_key_hash": "0xd4044c0f6183e29c9b0eac374056cee14aa5c239b811be978df8e9454d58cc94",
                    "type": "write_resource"
                },
                {
                    "data": null,
                    "handle": "0x1b854694ae746cdbd8d44186ca4929b2b337df21d1c74633be19b2710552fdca",
                    "key": "0x0619dc29a0aac8fa146714058e8dd6d2d0f3bdf5f6331907bf91f3acd81e6935",
                    "state_key_hash": "0x6e4b28d40f98a106a65163530924c0dcb40c1349d3aa915d108b4d6cfc1ddb19",
                    "type": "write_table_item",
                    "value": "0x70618c8b6a2770010000000000000000"
                },
                {
                    "data": null,
                    "handle": "0x2e46d98566716dc078ca858162169242fd0e45d7f2f8fb4799767c17e0af99bd",
                    "key": "0x4c1b416e4e3793724dabd2d10909689bd534159a2d4d4b605ec22d9e593bbad3",
                    "state_key_hash": "0x4696d45e724c41fee140ae0f15dcbe89107e5cd6723265e88efcb4bc1e978b2d",
                    "type": "delete_table_item"
                }
            ],
            "event_root_hash": "0x239bf6aedae7d54a532d2fbd13d2bb2228d9dd51135167fa2e8fc9b13f721c1a",
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
                        "account_address": "0x4c1b416e4e3793724dabd2d10909689bd534159a2d4d4b605ec22d9e593bbad3",
                        "creation_number": "0"
                    },
                    "sequence_number": "1",
                    "type": "0x1::account::CoinRegisterEvent"
                },
                {
                    "data": {
                        "amount": "50000"
                    },
                    "guid": {
                        "account_address": "0x4c1b416e4e3793724dabd2d10909689bd534159a2d4d4b605ec22d9e593bbad3",
                        "creation_number": "4"
                    },
                    "sequence_number": "0",
                    "type": "0x1::coin::DepositEvent"
                },
                {
                    "data": {
                        "amount_ld": "50000",
                        "coin_type": {
                            "account_address": "0xf22bede237a07e121b56d91a491eb7bcdfd1f5907926a9e58338f964a01b17fa",
                            "module_name": "0x6173736574",
                            "struct_name": "0x55534454"
                        },
                        "receiver": "0x4c1b416e4e3793724dabd2d10909689bd534159a2d4d4b605ec22d9e593bbad3"
                    },
                    "guid": {
                        "account_address": "0xf22bede237a07e121b56d91a491eb7bcdfd1f5907926a9e58338f964a01b17fa",
                        "creation_number": "6"
                    },
                    "sequence_number": "186072",
                    "type": "0xf22bede237a07e121b56d91a491eb7bcdfd1f5907926a9e58338f964a01b17fa::coin_bridge::ClaimEvent"
                }
            ],
            "expiration_timestamp_secs": "1685063040",
            "gas_unit_price": "100",
            "gas_used": "517",
            "hash": "0x434d4f991a70384d11f41ae1627089173f82f014db60c0c4b806afaf32384b49",
            "max_gas_amount": "1034",
            "payload": {
                "arguments": [],
                "function": "0xf22bede237a07e121b56d91a491eb7bcdfd1f5907926a9e58338f964a01b17fa::coin_bridge::claim_coin",
                "type": "entry_function_payload",
                "type_arguments": [
                    "0xf22bede237a07e121b56d91a491eb7bcdfd1f5907926a9e58338f964a01b17fa::asset::USDT"
                ]
            },
            "sender": "0x4c1b416e4e3793724dabd2d10909689bd534159a2d4d4b605ec22d9e593bbad3",
            "sequence_number": "0",
            "signature": {
                "public_key": "0x194e8ddc4fe79daa79f5e3de0e6ee499df8a331df28bb512e9a056871e2b9b45",
                "signature": "0x34e0a9bf2598698f46ac5b9b1f93deb5b3c3b6ccf417c844894629d6ec0a7e4ab77504998bb883a2ba1ea3d77d6ecf4fde3167c247861086d80c7bbdd892b705",
                "type": "ed25519_signature"
            },
            "state_change_hash": "0xe1e6651f1f5b03fcd76db010b7d229e1468aadc518764f5ffce2d7bbc1ae4ed3",
            "state_checkpoint_hash": null,
            "success": true,
            "timestamp": "1685063032234522",
            "type": "user_transaction",
            "version": "147811396",
            "vm_status": "Executed successfully"
        },
        {
            "accumulator_root_hash": "0xabb7ca17212b71b84e5e6da91f29f8e8edc5e41751fcb71d343eb595391fd1f4",
            "changes": [],
            "event_root_hash": "0x414343554d554c41544f525f504c414345484f4c4445525f4841534800000000",
            "gas_used": "0",
            "hash": "0x1b10f560cdf966f69e409b8190abc207a784afc0824ffd21e5dbbcd4f98b629a",
            "state_change_hash": "0xafb6e14fe47d850fd0a7395bcfb997ffacf4715e0f895cc162c218e4a7564bc6",
            "state_checkpoint_hash": "0x96d424f2d4b9cccfd8964ed8604b00069572f001382d55692c06e2bfcd2efdd1",
            "success": true,
            "timestamp": "1685063032234522",
            "type": "state_checkpoint_transaction",
            "version": "147811397",
            "vm_status": "Executed successfully"
        }
    ]
}
```
