---
description: >-
  Example code for the eth_getBlockByNumber json-rpc method. Сomplete guide on
  how to use eth_getBlockByNumber json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByNumber - Gnosis

#### Parameters

`QUANTITY|TAG` - hex string

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

`Boolean` - boolean

If true, returns the full transaction objects; if false, returns only the hashes of the transactions.

#### Request

```java
curl --location --request POST 'https://gno.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getBlockByNumber",
"params": ["0x68B3", true],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "author": "0x3870c57fbf1d7e49b154269331c7bc66c64d8857",
        "difficulty": "0xfffffffffffffffffffffffffffffffe",
        "extraData": "0xde830201018f5061726974792d457468657265756d86312e32392e30826c69",
        "gasLimit": "0x989680",
        "gasUsed": "0x0",
        "hash": "0xa8b5e26e336a9edecf9d616e865d690895788ca77981728fcf824a4462f2c4ca",
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "miner": "0x3870c57fbf1d7e49b154269331c7bc66c64d8857",
        "number": "0x68b3",
        "parentHash": "0x49f009a16d54974a73a776b240b179e4678e467c5cf18968735bd92470506b23",
        "receiptsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "signature": "0x95b8d61f40fac919e4ab2e73c0f607de6d555bde3f9897c0b1b0cee3827d0d8c3b86fc7bb1e074a8872d1523779620bafbfec2096a5dd9014a9fee79a560f01900",
        "size": "0x24b",
        "stateRoot": "0xefee062aa33118f7dde568f2e946e913ece9c78f3fe7fd04109d497b59463e02",
        "step": 307831686,
        "timestamp": "0x5bbdb19e",
        "totalDifficulty": "0x68b2ffffffffffffffffffffffffeda873c7",
        "transactions": [],
        "transactionsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "uncles": []
    }
}
```
