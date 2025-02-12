---
description: >-
  Example code for the eth_getTransactionReceipt json-rpc method. Сomplete guide
  on how to use eth_getTransactionReceipt json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionReceipt - Fantom

#### Parameters

`DATA` - hex string

32-byte hash of a transaction.

#### Request

```java
curl --location --request POST 'https://ftm.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionReceipt",
"params": ["0xd3a7420982975e8b161e7a049c582ee20900762f6c005dcccfd55d33a94d9b5f"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x00033bca0000046700d44a27301783f44016362a31ee066aa2a3ff82350783a9",
        "blockNumber": "0x3be8f6d",
        "contractAddress": null,
        "cumulativeGasUsed": "0x73720",
        "effectiveGasPrice": "0xd879b1fa9",
        "from": "0xbee08753d42db191f438979d3cb6a08d28d36555",
        "gasUsed": "0x17778",
        "logs": [],
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "status": "0x1",
        "to": "0x4992a76c6835f03afe73a20cec8445336daea05a",
        "transactionHash": "0xd3a7420982975e8b161e7a049c582ee20900762f6c005dcccfd55d33a94d9b5f",
        "transactionIndex": "0x3",
        "type": "0x2"
    }
}
```
