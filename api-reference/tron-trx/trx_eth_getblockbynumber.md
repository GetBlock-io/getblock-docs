---
description: >-
  Example code for the eth_getBlockByNumber json-rpc method. Сomplete guide on
  how to use eth_getBlockByNumber json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByNumber - TRON

#### Parameters

`QUANTITY` - QUANTITY

Integer of a block number, or the string "earliest", "latest"

`boolean` - boolean

If true it returns the full transaction objects, if false only the hashes of the transactions.

#### Request

```java
curl --location --request POST 'https://trx.getblock.io/mainnet/jsonrpc' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getBlockByNumber",
"params": ["0x25ad523", false],
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
            "0x92273e5a340d73687cbe3a9a98b47cdcb559f8c6c1f4a054b7c7cb08fceafcdb",
            "0x144addd6d19590a5eef5ae03d45a41b9628191993b33d37b450d7fdf89525926",
            "0x63b560ac1c707b10641ff675890641fba18a9aebf31738bedaa75ba2b953f9b2",
            "0x5a25a67bf0170ca2064edf2d4e10d5f95cc4a530d8a65fafd7335ca472cdceb0",
            "0xca87fdb68a7a43c714887bff930c8bf0d8eb323c8a9726d0a000eb7d733572ad",
            "0x5d20c2791048b3972a5055652108972b1b14b726a04690a7a514539cbb142bf1",
            "0xc2eb4ac2b0e7f9e433175eb80b70a30782fce3f0fd2a6a4d151926b1d95e0c71",
            "0x826f8523d35485acd07ac08c080402fcdb7b197b2e9f466fbbf336229c2fc656",
            "0xd8bfd634d9c280ca40f9767f1035535071406f1d1190fcd9d0d47b76553cd507",
            "0x0c967b72d491a57ccd9fe4303af3ba9551a4f4b7a6357b5c19f3a70e7d5aac9f",
            "0x5aeae316242a6da48e4e89a30c8d3f42b1934a2553b844df8f611a2be5713039",
            "0xd5145f3330182f10a1a2638b687e0f8167eb35589b463bda8f5be3314ca1bd54",
            "0x74a1dff3599bd30da1e0753a28aa86163c36a5ff8f859da94128eb93a1e253b1",
            "0xf154fd7629d2437d3013a87b6d23fca024229b539591ae54b97a04d2d732ad5f"
        ],
        "transactionsRoot": "0x00279614b13dbebfd3e96ac6ecb99fcf447a093a9e6a1a959aaa903235cda54f",
        "uncles": []
    }
}
```
