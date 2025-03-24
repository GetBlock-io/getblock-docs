---
description: >-
  Example code for the eth_getTransactionReceipt json-rpc method. Сomplete guide
  on how to use eth_getTransactionReceipt json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionReceipt - TRON

#### Parameters

`hash` - bytes 32

hash of a transaction

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionReceipt",
"params": ["0x9ff75c018157f4504adc34058510516c603569238e5c3d156b1f0b096856b1a1"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x00000000025ad523bf3d34c17ff81e2cf46824d38ff0373d172fd293ca7e651f",
        "blockNumber": "0x25ad523",
        "contractAddress": null,
        "cumulativeGasUsed": "0x92864",
        "effectiveGasPrice": "0x118",
        "from": "0xa346f2bd7d43a5d90b7c57a18196be96b2840e61",
        "gasUsed": "0x73bf",
        "logs": [
            {
                "address": "0xa614f803b6fd780986a42c78ec9c7f77e6ded13c",
                "blockHash": "0x00000000025ad523bf3d34c17ff81e2cf46824d38ff0373d172fd293ca7e651f",
                "blockNumber": "0x25ad523",
                "data": "0x0000000000000000000000000000000000000000000000000000000022a640dc",
                "logIndex": "0x1e",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000a346f2bd7d43a5d90b7c57a18196be96b2840e61",
                    "0x00000000000000000000000001eb1a2d0528dd1d99592cd7baa98f2603804f76"
                ],
                "transactionHash": "0x9ff75c018157f4504adc34058510516c603569238e5c3d156b1f0b096856b1a1",
                "transactionIndex": "0x3e"
            }
        ],
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "status": "0x1",
        "to": "0xa614f803b6fd780986a42c78ec9c7f77e6ded13c",
        "transactionHash": "0x9ff75c018157f4504adc34058510516c603569238e5c3d156b1f0b096856b1a1",
        "transactionIndex": "0x3e",
        "type": "0x0"
    }
}
```
