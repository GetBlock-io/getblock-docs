---
description: >-
  Example code for the eth_getTransactionReceipt json-rpc method. Сomplete guide
  on how to use eth_getTransactionReceipt json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionReceipt - Avalanche

#### Parameters

`DATA` - string

Hash of a transaction.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{
  "jsonrpc": "2.0",
  "method": "eth_getTransactionReceipt",
  "params": ["0x22dc79ee594717cf14a20f125906b658d88f595a4409afb71fd85dc0cb7c180c"],
  "id": "getblock.io"
}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x3e56c97d34f03b1369c351fa6c9f57c8bfa987c7da40964fab981303e0ef5849",
        "blockNumber": "0xca8de4",
        "contractAddress": null,
        "cumulativeGasUsed": "0xd2f05",
        "effectiveGasPrice": "0x163c8a3a22",
        "from": "0xa69f2954a5da5c16c99998588ea626290e887c18",
        "gasUsed": "0xcc45",
        "logs": [
            {
                "address": "0x625fc9bb971bb305a2ad63252665dcfe9098bee9",
                "blockHash": "0x3e56c97d34f03b1369c351fa6c9f57c8bfa987c7da40964fab981303e0ef5849",
                "blockNumber": "0xca8de4",
                "data": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                "logIndex": "0x14",
                "removed": false,
                "topics": [
                    "0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925",
                    "0x000000000000000000000000a69f2954a5da5c16c99998588ea626290e887c18",
                    "0x000000000000000000000000e54ca86531e17ef3616d22ca28b0d458b6c89106"
                ],
                "transactionHash": "0x22dc79ee594717cf14a20f125906b658d88f595a4409afb71fd85dc0cb7c180c",
                "transactionIndex": "0x3"
            }
        ],
        "logsBloom": "0x00000000000001000000000000000000000000008000000000000000000000000000004000000000000000000000000000000000000000000000000000200000000000000000000000000000000000001000000000000000000000020000000000000000000000000000000800004000000000000000000000000001000000004000000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000",
        "status": "0x1",
        "to": "0x625fc9bb971bb305a2ad63252665dcfe9098bee9",
        "transactionHash": "0x22dc79ee594717cf14a20f125906b658d88f595a4409afb71fd85dc0cb7c180c",
        "transactionIndex": "0x3",
        "type": "0x0"
    }
}
```
