---
description: >-
  Example code for the /v1/transactions/by_version/{version} json-rpc method.
  Сomplete guide on how to use /v1/transactions/by_version/{version} json-rpc in
  GetBlock.io Web3 documentation.
---

# /v1/transactions/by\_version/{version} - Aptos

#### Parameters

\-

#### Request

```java
curl --location --request GET 'https://apt.getblock.io/v1/transactions/by_version/147811396?' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
```

#### Response

```java
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
}
```
