---
description: >-
  Example code for the eth_getLogs json-rpc method. Сomplete guide on how to use
  eth_getLogs json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getLogs - Binance Smart Chain

#### Parameters

`Object` - hex string

Filter options object.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getLogs",
"params": [{"fromBlock": "0x1B13C84", "toBlock": "0x1B13C84", "address": "0xe9e7cea3dedca5984780bafc599bd69add087d56", "topics": []}],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": [
        {
            "address": "0xe9e7cea3dedca5984780bafc599bd69add087d56",
            "blockHash": "0x09304310ce27a9a6200b9a33fa4b9cbbd1c5394cdf6b599b4dc15c09be2250cb",
            "blockNumber": "0x1b13c84",
            "data": "0x0000000000000000000000000000000000000000000000102b95cd89e5de0000",
            "logIndex": "0x5",
            "removed": false,
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000c39978a25a9c52c8cc12c0d90e03cd6ab0c8a620",
                "0x0000000000000000000000008e744ec2795c8b836689d1b4ebe1489204357dac"
            ],
            "transactionHash": "0x638a31c10aaec9d09dfdab13e1aec121b2ec4fad2fbfcc37ee0fa24d227e8fee",
            "transactionIndex": "0x5"
        },
        {
            "address": "0xe9e7cea3dedca5984780bafc599bd69add087d56",
            "blockHash": "0x09304310ce27a9a6200b9a33fa4b9cbbd1c5394cdf6b599b4dc15c09be2250cb",
            "blockNumber": "0x1b13c84",
            "data": "0x00000000000000000000000000000000000000000000000bae5f766342dbfbed",
            "logIndex": "0x9",
            "removed": false,
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x0000000000000000000000004150f3439ad0540e219ec727a90bdbe5b51524c3",
                "0x0000000000000000000000008e744ec2795c8b836689d1b4ebe1489204357dac"
            ],
            "transactionHash": "0x68f3bd44659bda76bdd7109e62fa6f23a7381dc9b52353e007f4a5606a54d604",
            "transactionIndex": "0x6"
        },
        {
            "address": "0xe9e7cea3dedca5984780bafc599bd69add087d56",
            "blockHash": "0x09304310ce27a9a6200b9a33fa4b9cbbd1c5394cdf6b599b4dc15c09be2250cb",
            "blockNumber": "0x1b13c84",
            "data": "0x00000000000000000000000000000000000000000000003273baa7499616c000",
            "logIndex": "0x1f",
            "removed": false,
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000179755e1b74fd8738e77388337a738d774e3b451",
                "0x0000000000000000000000008e744ec2795c8b836689d1b4ebe1489204357dac"
            ],
            "transactionHash": "0x383cd92ad4e46e457f0fa2054d1e586f3d543ab628c80968366fcf10be71e3bb",
            "transactionIndex": "0xb"
        },
        {
            "address": "0xe9e7cea3dedca5984780bafc599bd69add087d56",
            "blockHash": "0x09304310ce27a9a6200b9a33fa4b9cbbd1c5394cdf6b599b4dc15c09be2250cb",
            "blockNumber": "0x1b13c84",
            "data": "0xfffffffffffffffffffffffffffffffffffffffffffbed8e1c6e89ee380c5462",
            "logIndex": "0x20",
            "removed": false,
            "topics": [
                "0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925",
                "0x000000000000000000000000179755e1b74fd8738e77388337a738d774e3b451",
                "0x00000000000000000000000010ed43c718714eb63d5aa57b78b54704e256024e"
            ],
            "transactionHash": "0x383cd92ad4e46e457f0fa2054d1e586f3d543ab628c80968366fcf10be71e3bb",
            "transactionIndex": "0xb"
        },
        {
            "address": "0xe9e7cea3dedca5984780bafc599bd69add087d56",
            "blockHash": "0x09304310ce27a9a6200b9a33fa4b9cbbd1c5394cdf6b599b4dc15c09be2250cb",
            "blockNumber": "0x1b13c84",
            "data": "0x0000000000000000000000000000000000000000000000240379b94994aa259e",
            "logIndex": "0x69",
            "removed": false,
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x0000000000000000000000004f3126d5de26413abdcf6948943fb9d0847d9818",
                "0x00000000000000000000000013f4ea83d0bd40e75c8222255bc855a974568dd4"
            ],
            "transactionHash": "0x617deb18941001a3b5e3fbf0120f72a1c3796b9560ad1cceeb0c340b35677bd3",
            "transactionIndex": "0x1c"
        },
        {
            "address": "0xe9e7cea3dedca5984780bafc599bd69add087d56",
            "blockHash": "0x09304310ce27a9a6200b9a33fa4b9cbbd1c5394cdf6b599b4dc15c09be2250cb",
            "blockNumber": "0x1b13c84",
            "data": "0x0000000000000000000000000000000000000000000000240379b94994aa259e",
            "logIndex": "0x6d",
            "removed": false,
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x00000000000000000000000013f4ea83d0bd40e75c8222255bc855a974568dd4",
                "0x0000000000000000000000008e744ec2795c8b836689d1b4ebe1489204357dac"
            ],
            "transactionHash": "0x617deb18941001a3b5e3fbf0120f72a1c3796b9560ad1cceeb0c340b35677bd3",
            "transactionIndex": "0x1c"
        },
        {
            "address": "0xe9e7cea3dedca5984780bafc599bd69add087d56",
            "blockHash": "0x09304310ce27a9a6200b9a33fa4b9cbbd1c5394cdf6b599b4dc15c09be2250cb",
            "blockNumber": "0x1b13c84",
            "data": "0x0000000000000000000000000000000000000000000000056bc75e2d63100000",
            "logIndex": "0x8a",
            "removed": false,
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000fe52532fee05fdd7b6b5b4d42b999a2cd246bec7",
                "0x0000000000000000000000000e9939a5ad925de47536d8298a095b095cc6055c"
            ],
            "transactionHash": "0x4bec8bc79494adc928795bf69f43fb0b5ed448d687607250f0ca9d2987d026dd",
            "transactionIndex": "0x26"
        },
        {
            "address": "0xe9e7cea3dedca5984780bafc599bd69add087d56",
            "blockHash": "0x09304310ce27a9a6200b9a33fa4b9cbbd1c5394cdf6b599b4dc15c09be2250cb",
            "blockNumber": "0x1b13c84",
            "data": "0x000000000000000000000000000000000000000000000010407fcf0f5f59c884",
            "logIndex": "0xa8",
            "removed": false,
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x0000000000000000000000004f3126d5de26413abdcf6948943fb9d0847d9818",
                "0x000000000000000000000000fd891b99668048c6a97a4841130116baf0224b24"
            ],
            "transactionHash": "0xe7720b2ef0025efcb9941fc9a32b93eaca01f926c14b7589a82a9e83aeebc82c",
            "transactionIndex": "0x38"
        },
        {
            "address": "0xe9e7cea3dedca5984780bafc599bd69add087d56",
            "blockHash": "0x09304310ce27a9a6200b9a33fa4b9cbbd1c5394cdf6b599b4dc15c09be2250cb",
            "blockNumber": "0x1b13c84",
            "data": "0x0000000000000000000000000000000000000000000000104289bfbab0695ac5",
            "logIndex": "0xe4",
            "removed": false,
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x0000000000000000000000007efaef62fddcca950418312c6c91aef321375a00",
                "0x00000000000000000000000058f876857a02d6762e0101bb5c46a8c1ed44dc16"
            ],
            "transactionHash": "0xc41a62b82ee253f0815f9c3a2214f38735553f91728ed6904a102344ac413dbd",
            "transactionIndex": "0x51"
        },
        {
            "address": "0xe9e7cea3dedca5984780bafc599bd69add087d56",
            "blockHash": "0x09304310ce27a9a6200b9a33fa4b9cbbd1c5394cdf6b599b4dc15c09be2250cb",
            "blockNumber": "0x1b13c84",
            "data": "0x0000000000000000000000000000000000000000000000056bc75e2d63100000",
            "logIndex": "0xf4",
            "removed": false,
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x0000000000000000000000006a02aeddb994cbb1d63d5fbe49a46205f192cf2f",
                "0x00000000000000000000000058f876857a02d6762e0101bb5c46a8c1ed44dc16"
            ],
            "transactionHash": "0x167612b55175327c53a1375f613df1c3ae0dce0ca8db38e3b9e674e38db371b7",
            "transactionIndex": "0x58"
        },
        {
            "address": "0xe9e7cea3dedca5984780bafc599bd69add087d56",
            "blockHash": "0x09304310ce27a9a6200b9a33fa4b9cbbd1c5394cdf6b599b4dc15c09be2250cb",
            "blockNumber": "0x1b13c84",
            "data": "0xffffffffffffffffffffffffffffffffffffffffffff85d712efc5f003efb5f7",
            "logIndex": "0xf5",
            "removed": false,
            "topics": [
                "0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925",
                "0x0000000000000000000000006a02aeddb994cbb1d63d5fbe49a46205f192cf2f",
                "0x00000000000000000000000010ed43c718714eb63d5aa57b78b54704e256024e"
            ],
            "transactionHash": "0x167612b55175327c53a1375f613df1c3ae0dce0ca8db38e3b9e674e38db371b7",
            "transactionIndex": "0x58"
        }
    ]
}
```
