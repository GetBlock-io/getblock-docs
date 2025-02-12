---
description: >-
  Example code for the eth_getBlockByHash json-rpc method. Сomplete guide on how
  to use eth_getBlockByHash json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByHash - TRON

#### Parameters

`DATA` - 32 bytes

hash of a block

`boolean` - boolean

If true it returns the full transaction objects, if false only the hashes of the transactions.

#### Request

```java
curl --location --request POST 'https://trx.getblock.io/mainnet/jsonrpc' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getBlockByHash",
"params": ["0x00000000025ad523bf3d34c17ff81e2cf46824d38ff0373d172fd293ca7e651f", true],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "baseFeePerGas": null,
        "difficulty": null,
        "extraData": null,
        "gasLimit": "0x236a8f8e0",
        "gasUsed": "0x1c2d7f",
        "hash": "0x00000000025ad523bf3d34c17ff81e2cf46824d38ff0373d172fd293ca7e651f",
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "miner": "0x1761716b76c6a3d885299c366826046c09b08d26",
        "mixHash": null,
        "nonce": null,
        "number": "0x25ad523",
        "parentHash": "0x00000000025ad5222c83c5331b677d87c71cf40a6d2a691738dda7136e7dc0f8",
        "receiptsRoot": null,
        "sha3Uncles": null,
        "size": "0xe110",
        "stateRoot": "0x",
        "timestamp": "0x17ff4639bb8",
        "totalDifficulty": null,
        "transactions": [
            {
                "blockHash": "0x00000000025ad523bf3d34c17ff81e2cf46824d38ff0373d172fd293ca7e651f",
                "blockNumber": "0x25ad523",
                "from": "0xa953f5f78af3cc2bd87ad72aa11afd8f1fd896f5",
                "gas": "0x0",
                "gasPrice": "0x118",
                "hash": "0x92273e5a340d73687cbe3a9a98b47cdcb559f8c6c1f4a054b7c7cb08fceafcdb",
                "input": "0x",
                "nonce": null,
                "r": "0xcb0b946a8a2f22c982acf5ae58e4f5cc4a38c7616ba3d822907ae4d33f59970f",
                "s": "0x38281a083000d64de653a20f6395c4e291a88f1d3ab746cf3188fa0869ab5f2f",
                "to": "0x914d56d523b8fd239bd8f5a1df290d0d5ba7d5e5",
                "transactionIndex": "0x0",
                "type": "0x0",
                "v": "0x1b",
                "value": "0x178f1e3a"
            },
            {
                "blockHash": "0x00000000025ad523bf3d34c17ff81e2cf46824d38ff0373d172fd293ca7e651f",
                "blockNumber": "0x25ad523",
                "from": "0x0b48984414cc0c6a8e599fb6e3bc11e599de2e24",
                "gas": "0x3927",
                "gasPrice": "0x118",
                "hash": "0x144addd6d19590a5eef5ae03d45a41b9628191993b33d37b450d7fdf89525926",
                "input": "0x",
                "nonce": null,
                "r": "0x203115ef3e77385bf311be90f6c61b0e3f5048ecc27d1b4b4539df4bacadc9aa",
                "s": "0x23dcda1c3a6a575d04c161f2d990d26232d2977be1de8848b95876f9a1ece307",
                "to": "0xa614f803b6fd780986a42c78ec9c7f77e6ded13c",
                "transactionIndex": "0x1",
                "type": "0x0",
                "v": "0x1b",
                "value": "0x0"
            }
        ],
        "transactionsRoot": "0x00279614b13dbebfd3e96ac6ecb99fcf447a093a9e6a1a959aaa903235cda54f",
        "uncles": []
    }
}
```
