---
description: >-
  Example code for the eth_getBlockByHash json-rpc method. Сomplete guide on how
  to use eth_getBlockByHash json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByHash - Fantom

#### Parameters

`DATA` - hex string

32-byte hash of a block.

`Boolean` - boolean

If true, returns the full transaction objects; if false, returns the transaction hashes.

#### Request

```java
curl --location --request POST 'https://ftm.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getBlockByHash",
"params": ["0x00033bca0000046700d44a27301783f44016362a31ee066aa2a3ff82350783a9", false],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "baseFeePerGas": "0x80c9e8743",
        "difficulty": "0x0",
        "epoch": "0x33bca",
        "extraData": "0x",
        "gasLimit": "0xffffffffffff",
        "gasUsed": "0xf6d23",
        "hash": "0x00033bca0000046700d44a27301783f44016362a31ee066aa2a3ff82350783a9",
        "logsBloom": "0x00000000000000000000000000000002000200000000000000008000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002000000000000000000000000100000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000000000000000000000800000000000800000200004000000000000000000000000000000000002000000",
        "miner": "0x0000000000000000000000000000000000000000",
        "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "nonce": "0x0000000000000000",
        "number": "0x3be8f6d",
        "parentHash": "0x00033bca0000045c6e477386025a29843c9e69226882e2a01128d4dd298b4c1d",
        "receiptsRoot": "0xdf2c18c77a6ed890e3e81d417b0704d0fa821a51f9dedcae2e977a98ddea2e72",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x1670",
        "stateRoot": "0x10e49fb5f42349010f458041e4bd569f0024a469f1978e4f9d561abb7ed92656",
        "timestamp": "0x646a7df4",
        "timestampNano": "0x17614253277addab",
        "totalDifficulty": "0x0",
        "transactions": [
            "0x01ba19c6921948bfce065d94cfcf1de29f733fb9f1090e6ab19f1d89cc4eac02",
            "0xc5f50402b3ae767fc34946593e1884eac094ed3e706353be27a8c774d48c4c2c",
            "0xb6a1b91c54e34696003c1ffa1acc631c3950cea6c76090bdd94f161da6009ff1",
            "0xd3a7420982975e8b161e7a049c582ee20900762f6c005dcccfd55d33a94d9b5f",
            "0x967d9effa12ce565231472d4c7577b1a218ba22a4c1ef3bf3abd4be1e9a1af89",
            "0x53a42db11ae32623b4e99493dde998d1d312b363d3754da02eaa1f8aa225aeda",
            "0x81901c7bec00d1e3caf70abfd83d3965cba6dee8485b95aadaf16a7419eaec40",
            "0xf09e54dec44b6e3e5733da3a9165ea2cd0626322dddbf0181c408fb3cc042ac0",
            "0x3b140d40b11e355a6b7547b7068231639a3765fd75bfae5a274eae69d2133cca"
        ],
        "transactionsRoot": "0xa528944956f8e82130b371548a4eefcf7ba8db1ce66d0f8c9d0d5f26fb2f409f",
        "uncles": []
    }
}
```
