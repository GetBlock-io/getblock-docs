---
description: >-
  Example code for the /v1/transactions/by_hash/{transaction_hash} json-rpc
  method. Сomplete guide on how to use
  /v1/transactions/by_hash/{transaction_hash} json-rpc in GetBlock.io Web3
  documentation.
---

# /v1/transactions/by\_hash/{transaction\_hash} - Aptos

This endpoint gets a transaction from the Aptos blockchain by its **transaction hash**. Every transaction has a unique hash that can be used to retrieve full details.


## Supported network

* Mainnet

## Parameter

<table>
  <tr>
   <td><strong>Parameter</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>In</strong>
   </td>
   <td><strong>Required</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>transaction_hash
   </td>
   <td>String
   </td>
   <td>Path
   </td>
   <td>Yes
   </td>
   <td>Hash of the transaction
   </td>
  </tr>
</table>



## Request

**Base URL**

```bash
https://go.getblock.io/<ACCESS_TOKEN>
```

**Example(cURL)**


```curl
curl --location 'https://go.getblock.io/<ACCESS_TOKEN>/v1/transactions/by_hash/0xd797b944ed8657406a1b09a5928048093399fc0a2f576d3e57c0f9cedbf95c4a'
```

## Response Example

```json
{
    "version": "3556737308",
    "hash": "0x4c3248d4c2f3a5ca1617e43e69332c66586478871ee44634cdb4554fe537dff6",
    "state_change_hash": "0x50148eff819eeb093efbe1cb56936ba02b3169db3343f5df0cb47aba016ea227",
    "event_root_hash": "0x1c432ce28a587287ef8abf14f025b78f5e288e886f16f217919cd2e35274f5f3",
    "state_checkpoint_hash": null,
    "gas_used": "195",
    "success": false,
    "vm_status": "Move abort in 0x31deb490728ae8e1a89675ee4c1877af8e5849c526d2c9a64238c49d8ece08a0::fa: E_NO_PROFIT(0x385): ",
    "accumulator_root_hash": "0xdc354eb8f4a96e9267bab2727240965becd154e35a21fede212af96f56333955",
    "changes": [
        {
            "address": "0xa",
            "state_key_hash": "0x1db5441d8fa4229c5844f73fd66da4ad8176cb8793d8b3a7f6ca858722030043",
            "data": {
                "type": "0x1::coin::PairedCoinType",
                "data": {
                    "type": {
                        "account_address": "0x1",
                        "module_name": "0x6170746f735f636f696e",
                        "struct_name": "0x4170746f73436f696e"
                    }
                }
            },
            "type": "write_resource"
        },
        {
            "address": "0xa",
            "state_key_hash": "0x1db5441d8fa4229c5844f73fd66da4ad8176cb8793d8b3a7f6ca858722030043",
            "data": {
                "type": "0x1::coin::PairedFungibleAssetRefs",
                "data": {
                    "burn_ref_opt": {
                        "vec": [
                            {
                                "metadata": {
                                    "inner": "0xa"
                                }
                            }
                        ]
                    },
                    "mint_ref_opt": {
                        "vec": [
                            {
                                "metadata": {
                                    "inner": "0xa"
                                }
                            }
                        ]
                    },
                    "transfer_ref_opt": {
                        "vec": [
                            {
                                "metadata": {
                                    "inner": "0xa"
                                }
                            }
                        ]
                    }
                }
            },
            "type": "write_resource"
        },
        {
            "address": "0xa",
            "state_key_hash": "0x1db5441d8fa4229c5844f73fd66da4ad8176cb8793d8b3a7f6ca858722030043",
            "data": {
                "type": "0x1::fungible_asset::ConcurrentSupply",
                "data": {
                    "current": {
                        "max_value": "340282366920938463463374607431768211455",
                        "value": "25726501655234340"
                    }
                }
            },
            "type": "write_resource"
        },
        {
            "address": "0xa",
            "state_key_hash": "0x1db5441d8fa4229c5844f73fd66da4ad8176cb8793d8b3a7f6ca858722030043",
            "data": {
                "type": "0x1::fungible_asset::Metadata",
                "data": {
                    "decimals": 8,
                    "icon_uri": "",
                    "name": "Aptos Coin",
                    "project_uri": "",
                    "symbol": "APT"
                }
            },
            "type": "write_resource"
        },
        {
            "address": "0xa",
            "state_key_hash": "0x1db5441d8fa4229c5844f73fd66da4ad8176cb8793d8b3a7f6ca858722030043",
            "data": {
                "type": "0x1::object::ObjectCore",
                "data": {
                    "allow_ungated_transfer": true,
                    "guid_creation_num": "1125899906842625",
                    "owner": "0x1",
                    "transfer_events": {
                        "counter": "0",
                        "guid": {
                            "id": {
                                "addr": "0xa",
                                "creation_num": "1125899906842624"
                            }
                        }
                    }
                }
            },
            "type": "write_resource"
        },
        {
            "address": "0xa",
            "state_key_hash": "0x1db5441d8fa4229c5844f73fd66da4ad8176cb8793d8b3a7f6ca858722030043",
            "data": {
                "type": "0x1::primary_fungible_store::DeriveRefPod",
                "data": {
                    "metadata_derive_ref": {
                        "self": "0xa"
                    }
                }
            },
            "type": "write_resource"
        },
        {
            "address": "0x60d8ef2e962b1d1b4f602c72e23eed6fbba86eefb41853a7688073fc918d912",
            "state_key_hash": "0x222b15ff81f90d75937ef6012807f0de0e0d488ccb5a229e55a954bc7870c75d",
            "data": {
                "type": "0x1::fungible_asset::FungibleStore",
                "data": {
                    "balance": "566263899",
                    "frozen": false,
                    "metadata": {
                        "inner": "0xa"
                    }
                }
            },
            "type": "write_resource"
        },
        {
            "address": "0x60d8ef2e962b1d1b4f602c72e23eed6fbba86eefb41853a7688073fc918d912",
            "state_key_hash": "0x222b15ff81f90d75937ef6012807f0de0e0d488ccb5a229e55a954bc7870c75d",
            "data": {
                "type": "0x1::object::ObjectCore",
                "data": {
                    "allow_ungated_transfer": false,
                    "guid_creation_num": "1125899906842625",
                    "owner": "0x31deb490728ae8e1a89675ee4c1877af8e5849c526d2c9a64238c49d8ece08a0",
                    "transfer_events": {
                        "counter": "0",
                        "guid": {
                            "id": {
                                "addr": "0x60d8ef2e962b1d1b4f602c72e23eed6fbba86eefb41853a7688073fc918d912",
                                "creation_num": "1125899906842624"
                            }
                        }
                    }
                }
            },
            "type": "write_resource"
        },
        {
            "address": "0x95a0fd88b11286d4790c253b39c6678b798c54b54b63bfd2bfe042411ee2e3e8",
            "state_key_hash": "0xf389cd5138744eb2c2c40a6b708ff717cb7a04bec7c9c1b9b8847857eaf617cd",
            "data": {
                "type": "0x1::account::Account",
                "data": {
                    "authentication_key": "0x95a0fd88b11286d4790c253b39c6678b798c54b54b63bfd2bfe042411ee2e3e8",
                    "coin_register_events": {
                        "counter": "0",
                        "guid": {
                            "id": {
                                "addr": "0x95a0fd88b11286d4790c253b39c6678b798c54b54b63bfd2bfe042411ee2e3e8",
                                "creation_num": "0"
                            }
                        }
                    },
                    "guid_creation_num": "2",
                    "key_rotation_events": {
                        "counter": "0",
                        "guid": {
                            "id": {
                                "addr": "0x95a0fd88b11286d4790c253b39c6678b798c54b54b63bfd2bfe042411ee2e3e8",
                                "creation_num": "1"
                            }
                        }
                    },
                    "rotation_capability_offer": {
                        "for": {
                            "vec": []
                        }
                    },
                    "sequence_number": "1680",
                    "signer_capability_offer": {
                        "for": {
                            "vec": []
                        }
                    }
                }
            },
            "type": "write_resource"
        }
    ],
    "sender": "0x95a0fd88b11286d4790c253b39c6678b798c54b54b63bfd2bfe042411ee2e3e8",
    "sequence_number": "1679",
    "max_gas_amount": "5000",
    "gas_unit_price": "100",
    "expiration_timestamp_secs": "1760341246",
    "payload": {
        "function": "0x31deb490728ae8e1a89675ee4c1877af8e5849c526d2c9a64238c49d8ece08a0::lesserafim_3::crazy",
        "type_arguments": [
            "0x1::aptos_coin::AptosCoin",
            "0x111ae3e5bc816a5e63c2da97d0aa3886519e0cd5e4b046659fa35796bd11542a::stapt_token::StakedApt",
            "0xf22bede237a07e121b56d91a491eb7bcdfd1f5907926a9e58338f964a01b17fa::asset::USDC",
            "0x159df6b7689437016108a019fd5bef736bac692b6d4a1f10c941f6fbb9a74ca6::oft::CakeOFT",
            "0x8d87a65ba30e09357fa2edea2c80dbac296e5dec2b18287113500b902942929d::celer_coin_manager::UsdcCoin",
            "0x1::aptos_coin::AptosCoin",
            "u64",
            "u64",
            "u64",
            "u64",
            "u64"
        ],
        "arguments": [
            "0x010f0a0101",
            "0x01010101010000",
            [
                "25",
                "10",
                "30",
                "25",
                "25"
            ],
            [
                {
                    "inner": "0xa"
                },
                {
                    "inner": "0xa"
                },
                {
                    "inner": "0xa"
                },
                {
                    "inner": "0xa"
                },
                {
                    "inner": "0xa"
                },
                {
                    "inner": "0xa"
                }
            ],
            [
                "0x1",
                "0x5669f388059383ab806e0dfce92196304205059874fd845944137d96bbdfc8de",
                "0x1",
                "0x1",
                "0x1"
            ],
            [
                false,
                true,
                false,
                true,
                false
            ],
            "28452012"
        ],
        "type": "entry_function_payload"
    },
    "signature": {
        "sender": {
            "public_key": "0x1e9d94603ebd6b4a1d38e760be2b0523f09fb1f59e1e43c657f0ab3ef6205427",
            "signature": "0x05f71501658fdc40b6f42f71d6c298a378434a5825af5bc0efcc15aba2aa38625064612139299f43ae40e710bad18ce0b9935dee00b705285a8f73318f06a301",
            "type": "ed25519_signature"
        },
        "secondary_signer_addresses": [],
        "secondary_signers": [],
        "fee_payer_address": "0x31deb490728ae8e1a89675ee4c1877af8e5849c526d2c9a64238c49d8ece08a0",
        "fee_payer_signer": {
            "public_key": "0x17dfb6544cf49621c75af067528755a38061bdd210342f8bdf565bcdf561f056",
            "signature": "0x7866a52d642e7442671d0614cf7b201a29bdbd8b709ea59462d32917c368f596d771ab6592d1659952970cb277a35d536d0966a754c2cd610858f8c04eb93e08",
            "type": "ed25519_signature"
        },
        "type": "fee_payer_signature"
    },
    "replay_protection_nonce": null,
    "events": [
        {
            "guid": {
                "creation_number": "0",
                "account_address": "0x0"
            },
            "sequence_number": "0",
            "type": "0x1::transaction_fee::FeeStatement",
            "data": {
                "execution_gas_units": "130",
                "io_gas_units": "65",
                "storage_fee_octas": "0",
                "storage_fee_refund_octas": "0",
                "total_charge_gas_units": "195"
            }
        }
    ],
    "timestamp": "1760341227055115",
    "type": "user_transaction"
}

```

## Response parameter Definition

<table>
  <tr>
   <td><strong>Field</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>version
   </td>
   <td>String
   </td>
   <td>Global transaction version number.
   </td>
  </tr>
  <tr>
   <td>hash
   </td>
   <td>String
   </td>
   <td>Transaction hash used for lookup.
   </td>
  </tr>
  <tr>
   <td>state_change_hash
   </td>
   <td>String
   </td>
   <td>Hash of all state changes from this transaction.
   </td>
  </tr>
  <tr>
   <td>event_root_hash
   </td>
   <td>String
   </td>
   <td>Merkle root hash of emitted events.
   </td>
  </tr>
  <tr>
   <td>state_checkpoint_hash
   </td>
   <td>String
   </td>
   <td>State checkpoint hash for this transaction.
   </td>
  </tr>
  <tr>
   <td>gas_used
   </td>
   <td>String
   </td>
   <td>Amount of gas consumed.
   </td>
  </tr>
  <tr>
   <td>success
   </td>
   <td>Boolean
   </td>
   <td>Whether execution succeeded.
   </td>
  </tr>
  <tr>
   <td>vm_status
   </td>
   <td>String
   </td>
   <td>Aptos VM execution result.
   </td>
  </tr>
  <tr>
   <td>accumulator_root_hash
   </td>
   <td>String
   </td>
   <td>Ledger accumulator root hash.
   </td>
  </tr>
  <tr>
   <td>changes
   </td>
   <td>Array
   </td>
   <td>State changes caused (may be empty).
   </td>
  </tr>
  <tr>
   <td>timestamp
   </td>
   <td>String
   </td>
   <td>Unix timestamp in microseconds.
   </td>
  </tr>
  <tr>
   <td>block_end_info
   </td>
   <td>Object
   </td>
   <td>Block execution metadata.
   </td>
  </tr>
  <tr>
   <td>type
   </td>
   <td>String
   </td>
   <td>Transaction type (e.g., user_transaction, block_epilogue_transaction).
   </td>
  </tr>
</table>

## Use cases

This method can be used for:

* Retrieve any transaction directly by hash (common for explorers and wallets).
* Debug and verify contract execution results.
* Cross-reference hash → version → block for indexing.
* Track system and user transactions separately.


## Code example

**Node(axios)**

```js
const axios = require('axios');

let config = {
  method: 'get',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.io/<ACCESS_TOKEN>/v1/transactions/by_hash/0xd797b944ed8657406a1b09a5928048093399fc0a2f576d3e57c0f9cedbf95c4a',
};

axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});


```

**Python(Request)**


```python
import requests

url = "https://go.getblock.io/<ACCESS_TOKEN>/v1/transactions/by_hash/0xd797b944ed8657406a1b09a5928048093399fc0a2f576d3e57c0f9cedbf95c4a"
response = requests.request("GET", url, headers=headers, data=payload)

print(response.text)
```



## Error handling

<table>
  <tr>
   <td><strong>Status Code</strong>
   </td>
   <td><strong>Error Message</strong>
   </td>
   <td><strong>Cause</strong>
   </td>
  </tr>
  <tr>
   <td><strong>400</strong>
   </td>
   <td>Invalid transaction hash
   </td>
   <td>Hash format is invalid (not hex or malformed).
   </td>
  </tr>
  <tr>
   <td><strong>403</strong>
   </td>
   <td>Forbidden
   </td>
   <td>Missing or invalid &lt;ACCESS_TOKEN>.
   </td>
  </tr>
  <tr>
   <td><strong>500</strong>
   </td>
   <td>Internal server error
   </td>
   <td>Node or network issue; retry later.
   </td>
  </tr>
</table>


## Integration with Web3

By integrating `/v1/transactions/by_hash/{txn_hash}`, developers can:

* Fetch transactions directly from wallet or explorer UIs.

* Debug dApps by verifying hash results against VM status.

* Enable search bars in explorers where users paste transaction hashes.

* Support analytics pipelines that track txns by hash across modules.
