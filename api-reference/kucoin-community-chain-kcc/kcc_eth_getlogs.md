---
description: >-
  Example code for the eth_getLogs json-rpc method. Сomplete guide on how to use
  eth_getLogs json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getLogs - KuCoin Community Chain

#### Parameters

`Object` - object

The filter options

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getLogs",
"params": [{"topics": []}],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": [
        {
            "address": "0x4200000000000000000000000000000000000006",
            "blockHash": "0xe5ed6f0c64f6a12eb0922189df036d646e5dba67e86498eed722c1fa6ab3fb54",
            "blockNumber": "0x2008e3",
            "data": "0x000000000000000000000000000000000000000000000000000277195bb68f80",
            "logIndex": "0x0",
            "removed": false,
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x00000000000000000000000057fe8051b897a14b4cbf84c22e7eeea0b30ba903",
                "0x0000000000000000000000004200000000000000000000000000000000000011"
            ],
            "transactionHash": "0x6e08b6a86fcdf34d1676625f943c7a9232e0d1173840f4d3aee7e930853602a6",
            "transactionIndex": "0x0"
        },
        {
            "address": "0x25e1c58040f27ecf20bbd4ca83a09290326896b3",
            "blockHash": "0xe5ed6f0c64f6a12eb0922189df036d646e5dba67e86498eed722c1fa6ab3fb54",
            "blockNumber": "0x2008e3",
            "data": "0x",
            "logIndex": "0x3",
            "removed": false,
            "topics": [
                "0xfe25c73e3b9089fac37d55c4c7efcba6f04af04cebd2fc4d6d7dbb07e1e5234f",
                "0x00000000000000000000000000000000000000000000009ce3f10dcf9b308000"
            ],
            "transactionHash": "0x6e08b6a86fcdf34d1676625f943c7a9232e0d1173840f4d3aee7e930853602a6",
            "transactionIndex": "0x0"
        }
    ]
}
```
