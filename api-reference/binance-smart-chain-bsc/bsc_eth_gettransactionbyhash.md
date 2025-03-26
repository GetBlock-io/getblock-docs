---
description: >-
  Example code for the eth_getTransactionByHash json-rpc method. Сomplete guide
  on how to use eth_getTransactionByHash json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionByHash - Binance Smart Chain

#### Parameters

`DATA` - hex string

32-byte transaction hash.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionByHash",
"params": ["0xfdfe964c83a3b5b4625697522c7e8f4107bb7fbcd869022a4555ff554cc1504f"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x6e6a058ac35224a9842a8010042ddc93dea2f073260a8d79cd3368a232c3e7ed",
        "blockNumber": "0x1afe984",
        "from": "0x2465176c461afb316ebc773c61faee85a6515daa",
        "gas": "0x7fffffffffffffff",
        "gasPrice": "0x0",
        "hash": "0xfdfe964c83a3b5b4625697522c7e8f4107bb7fbcd869022a4555ff554cc1504f",
        "input": "0xf340fa010000000000000000000000002465176c461afb316ebc773c61faee85a6515daa",
        "nonce": "0x108cf7",
        "r": "0x5e5781ba829b77aca8b9851dedc9e0a8bfb429dd8b62f395d74b121a706c0a59",
        "s": "0x206de0cfb982e3955bb4ad0b05ad4b91259a3944fed15c660511c434a78ab148",
        "to": "0x0000000000000000000000000000000000001000",
        "transactionIndex": "0x99",
        "type": "0x0",
        "v": "0x93",
        "value": "0x6a186b5096543c"
    }
}
```
