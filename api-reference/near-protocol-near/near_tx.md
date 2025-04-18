---
description: >-
  Example code for the tx json-rpc method. Сomplete guide on how to use tx
  json-rpc in GetBlock.io Web3 documentation.
---

# tx - NEAR Protocol

#### Parameters

`hash` - string

transaction hash

`id` - string

sender account id

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "tx",
"params": [6zgh2u9DqHHiXzdy9ouTP7oGky2T4nugqzqt9wJZwNFm, sender.testnet],
"id": "getblock.io"}'
```

#### Response

```java
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "status": {
            "SuccessValue": ""
        },
        "transaction": {
            "signer_id": "sender.testnet",
            "public_key": "ed25519:Gowpa4kXNyTMRKgt5W7147pmcc2PxiFic8UHW9rsNvJ6",
            "nonce": 15,
            "receiver_id": "receiver.testnet",
            "actions": [
                {
                    "Transfer": {
                        "deposit": "1000000000000000000000000"
                    }
                }
            ],
            "signature": "ed25519:3168QMdTpcwHvM1dmMYBc8hg9J3Wn8n7MWBSE9WrEpns6P5CaY87RM6k4uzyBkQuML38CZhU18HzmQEevPG1zCvk",
            "hash": "6zgh2u9DqHHiXzdy9ouTP7oGky2T4nugqzqt9wJZwNFm"
        },
        "transaction_outcome": {
            "proof": [
                {
                    "hash": "F7mL76CMdfbdZ3xCehVGNh1fCyaR37gr3MeGX3EZkiVf",
                    "direction": "Right"
                }
            ],
            "block_hash": "ADTMLVtkhsvzUxuf6m87Pt1dnF5vi1yY7ftxmNpFx7y",
            "id": "6zgh2u9DqHHiXzdy9ouTP7oGky2T4nugqzqt9wJZwNFm",
            "outcome": {
                "logs": [],
                "receipt_ids": [
                    "3dMfwczW5GQqXbD9GMTnmf8jy5uACxG6FC5dWxm3KcXT"
                ],
                "gas_burnt": 223182562500,
                "tokens_burnt": "22318256250000000000",
                "executor_id": "sender.testnet",
                "status": {
                    "SuccessReceiptId": "3dMfwczW5GQqXbD9GMTnmf8jy5uACxG6FC5dWxm3KcXT"
                }
            }
        },
        "receipts_outcome": [
            {
                "proof": [
                    {
                        "hash": "6h95oEd7ih62KXfyPT4zsZYont4qy9sWEXc5VQVDhqtG",
                        "direction": "Right"
                    },
                    {
                        "hash": "6DnibgZk1T669ZprcehUy1GpCSPw1kjzXRGu69nSaUNn",
                        "direction": "Right"
                    }
                ],
                "block_hash": "GgFTVr33r4MrpAiHc9mr8TZqLnpZAX1BaZTNvhBnciy2",
                "id": "3dMfwczW5GQqXbD9GMTnmf8jy5uACxG6FC5dWxm3KcXT",
                "outcome": {
                    "logs": [],
                    "receipt_ids": [
                        "46KYgN8ddxs4Qy8C7BDQH49XUfcYZsaQmAvdU1nfcL9V"
                    ],
                    "gas_burnt": 223182562500,
                    "tokens_burnt": "22318256250000000000",
                    "executor_id": "receiver.testnet",
                    "status": {
                        "SuccessValue": ""
                    }
                }
            },
            {
                "proof": [
                    {
                        "hash": "CD9Y7Bw3MSFgaPZzpc1yP51ajhGDCAsR61qXcMNcRoHf",
                        "direction": "Left"
                    }
                ],
                "block_hash": "EGAgKuW6Bd6QKYSaxAkx2pPGmnjrjAcq4UpoUiqMXvPH",
                "id": "46KYgN8ddxs4Qy8C7BDQH49XUfcYZsaQmAvdU1nfcL9V",
                "outcome": {
                    "logs": [],
                    "receipt_ids": [],
                    "gas_burnt": 0,
                    "tokens_burnt": "0",
                    "executor_id": "sender.testnet",
                    "status": {
                        "SuccessValue": ""
                    }
                }
            }
        ]
    }
}
```
