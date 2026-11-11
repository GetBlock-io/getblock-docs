---
description: >-
  Example code for the cosmos/tx/v1beta1/txs/{hash} REST method. Complete
  guide on how to use cosmos/tx/v1beta1/txs/{hash} REST method in GetBlock
  Web3 documentation.
---

# /cosmos/tx/v1beta1/txs/{hash} - Akash

Returns a decoded transaction and its response by hash via the Cosmos tx service.

## Endpoint

```
GET /cosmos/tx/v1beta1/txs/{hash}
```

## Path Parameters

| Parameter | Type   | Description      |
| --------- | ------ | ---------------- |
| hash      | string | Transaction hash |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/tx/v1beta1/txs/84CA9097D27B598FC494BD6CBEF538A4B5EC8344155358B12950374B467EAF9E"
```
{% endcode %}

## Response

```json
{
    "tx": {
        "body": {
            "messages": [
                {
                    "@type": "/cosmwasm.wasm.v1.MsgExecuteContract",
                    "sender": "akash1qafvet3v5nlkqdrlrkayy0eenq80aprqvj6nap",
                    "contract": "akash1436kxs0w2es6xlqpp9rd35e3d0cjnw4sv8j3a7483sgks29jqwgst0v7cu",
                    "msg": {
                        "update_price_feed": {
                            "vaa": "UE5BVQEAAAABJAEAAAABAwF1Y7Aix3lwYSSyhBA/HkwVSKD+Rwgkc8uUcwivjswAfgXMehGM6rJLm6Lx..."
                        }
                    },
                    "funds": [
                        {
                            "denom": "uakt",
                            "amount": "1"
                        }
                    ]
                }
            ],
            "memo": "akash price update",
            "timeout_height": "0",
            "unordered": true,
            "timeout_timestamp": "2026-09-22T18:11:01.247Z",
            "extension_options": [],
            "non_critical_extension_options": []
        },
        "auth_info": {
            "signer_infos": [
                {
                    "public_key": {
                        "@type": "/cosmos.crypto.secp256k1.PubKey",
                        "key": "A66o8QsxEpEk75TX3OyQum83jjPRBEYxtLVefsCWYvy6"
                    },
                    "mode_info": {
                        "single": {
                            "mode": "SIGN_MODE_DIRECT"
                        }
                    },
                    "sequence": "0"
                }
            ],
            "fee": {
                "amount": [
                    {
                        "denom": "uakt",
                        "amount": "10586"
                    }
                ],
                "gas_limit": "423435",
                "payer": "",
                "granter": ""
            },
            "tip": null
        },
        "signatures": [
            "6WDNNdy8R5K4gxBH+bJK5q0WMD9TqNLQtuCaUVDztY8lVdXIWlYgV5whnDUhWGkGzVwRJ8i41iOHQANepR2Oxw=="
        ]
    },
    "tx_response": {
        "height": "28742282",
        "txhash": "84CA9097D27B598FC494BD6CBEF538A4B5EC8344155358B12950374B467EAF9E",
        "codespace": "",
        "code": 0,
        "data": "122E0A2C2F636F736D7761736D2E7761736D2E76312E4D736745786563757465436F6E7472616374526573706F6E7365",
        "raw_log": "",
        "logs": [],
        "info": "",
        "gas_wanted": "423435",
        "gas_used": "302162",
        "tx": {
            "@type": "/cosmos.tx.v1beta1.Tx",
            "body": {
                "messages": [
                    {
                        "@type": "/cosmwasm.wasm.v1.MsgExecuteContract",
                        "sender": "akash1qafvet3v5nlkqdrlrkayy0eenq80aprqvj6nap",
                        "contract": "akash1436kxs0w2es6xlqpp9rd35e3d0cjnw4sv8j3a7483sgks29jqwgst0v7cu",
                        "msg": {
                            "update_price_feed": {
                                "vaa": "UE5BVQEAAAABJAEAAAABAwF1Y7Aix3lwYSSyhBA/HkwVSKD+Rwgkc8uUcwivjswAfgXMehGM6rJLm6Lx..."
                            }
                        },
                        "funds": [
                            {
                                "denom": "uakt",
                                "amount": "1"
                            }
                        ]
                    }
                ],
                "memo": "akash price update",
                "timeout_height": "0",
                "unordered": true,
                "timeout_timestamp": "2026-09-22T18:11:01.247Z",
                "extension_options": [],
                "non_critical_extension_options": []
            },
            "auth_info": {
                "signer_infos": [
                    {
                        "public_key": {
                            "@type": "/cosmos.crypto.secp256k1.PubKey",
                            "key": "A66o8QsxEpEk75TX3OyQum83jjPRBEYxtLVefsCWYvy6"
                        },
                        "mode_info": {
                            "single": {
                                "mode": "SIGN_MODE_DIRECT"
                            }
                        },
                        "sequence": "0"
                    }
                ],
                "fee": {
                    "amount": [
                        {
                            "denom": "uakt",
                            "amount": "10586"
                        }
                    ],
                    "gas_limit": "423435",
                    "payer": "",
                    "granter": ""
                },
                "tip": null
            },
            "signatures": [
                "6WDNNdy8R5K4gxBH+bJK5q0WMD9TqNLQtuCaUVDztY8lVdXIWlYgV5whnDUhWGkGzVwRJ8i41iOHQANepR2Oxw=="
            ]
        },
        "timestamp": "2026-09-22T18:08:00Z",
        "events": [
            {
                "type": "coin_spent",
                "attributes": [
                    {
                        "key": "spender",
                        "value": "akash1qafvet3v5nlkqdrlrkayy0eenq80aprqvj6nap",
                        "index": true
                    }
                ]
            }
        ]
    }
}
```

## Response Fields

| Field        | Type   | Description                            |
| ------------ | ------ | -------------------------------------- |
| tx           | object | Decoded transaction                    |
| tx\_response | object | Execution response (height, code, gas) |

## Use Cases

* **Receipts**: Fetch a tx by hash over REST

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
