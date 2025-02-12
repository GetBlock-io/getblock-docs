---
description: >-
  Example code for the eth_getBlockByHash json-rpc method. Сomplete guide on how
  to use eth_getBlockByHash json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByHash - Ethereum Classic

#### Parameters

`DATA` - string

32-byte hash of a block.

`verbose` - Boolean

If true, returns the full transaction objects; if false, returns the transaction hashes.

#### Request

```java
curl --location --request POST 'https://etc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getBlockByHash",
"params": ["0xbb50cb8d2d5698c1b10f1a845360ecf521ff18b08f71664a4639b6bdbb08fb38", false],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "difficulty": "0x11a85f2d724c0",
        "extraData": "0x657a696c2e6d65",
        "gasLimit": "0x96a324",
        "gasUsed": "0x92647",
        "hash": "0xbb50cb8d2d5698c1b10f1a845360ecf521ff18b08f71664a4639b6bdbb08fb38",
        "logsBloom": "0x00000000040000000000000000000100000000000100000000800800000000000000000000000000001000040000000000101000000000400000005000000010000100000000000000000000000000000000000000000000000000000000000000400000000000000000004080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000080000000000000000000080000000000010000000000000000000000000208000000000004000000000000000000000000000000001020000000000000000000000000000000000000000000000000000000000000000000000000080000",
        "miner": "0xb68ed353ea1421668483c46dc9b872aa09ef582a",
        "mixHash": "0xbfd0d3ea9f1784163d5baa89ca0e65749aa2371bb868a3e6d14c7f8fd795262e",
        "nonce": "0x4182ee1f71c2d7ba",
        "number": "0xcdf167",
        "parentHash": "0x2bd43d26217f9e6b6b91ba7e0f7b9db2b98c8fa12ebb5d673b090c4379eb6bb7",
        "receiptsRoot": "0x2073c879d7bad96c328d15619934880c5273d0c07bc42f94af0ed133bd1e4592",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0xe9e",
        "stateRoot": "0xcedf03d7df9ae5707a0bba93bdc4ce17485207254627efff695879c30f6f6fbf",
        "timestamp": "0x6139f68b",
        "totalDifficulty": "0x4ca722cd36ab8b5d6c",
        "transactions": [
            "0x01736b023300e297e3b3c7c13ab5db49c7c0be768d48790f46dd7baeefa7b047",
            "0xcdd15a0a55ce9a39a831057240191becbb9d39342e5773236d82afcc7923e08d",
            "0xcc5d7f84747e5a141a79f718d0b6d56544842638d4d150666e078134fe9bca07",
            "0xf23ea98db686d8199f30cb164683e83e4c9bad82fdb19c403ced93df7accef93",
            "0x5d3d8392a41037591db7eed2da4ec7bd7e1ac5be28a3ba057118995c4b7c3aa1",
            "0xc88c630c4c6b2e02a8432a71d00e77f3f009e3be426fcc3b31e82ffe379cfa6e",
            "0x9f08c1536fe557f85b41efd5a1763882fb1f2baba78a2f6a6d130acbf9981923",
            "0x03f3e11e250122ef8be4c23ea968d75521d98676ac2d1e0df0fbf5e6362ba9bd",
            "0x81bcb38ea94be6e36b4ff488e58a694d55998fa9342ac01d3669c23f42797ed3",
            "0xb44030bd3481dc0b7416ac7958babb812c70147b9825494bc1091a921684e7c4"
        ],
        "transactionsRoot": "0x76d8e6043cb8560de2fa9fba75acfbd99987c54c74b52b3b82bc78d407e3dbde",
        "uncles": []
    }
}
```
