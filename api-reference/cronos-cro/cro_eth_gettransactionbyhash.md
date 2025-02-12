---
description: >-
  Example code for the eth_getTransactionByHash json-rpc method. Сomplete guide
  on how to use eth_getTransactionByHash json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionByHash - Cronos

#### Parameters

`DATA` - hex string

32-byte transaction hash.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionByHash",
"params": ["0x05595f42e607777285b2e2e93f69ea5b20e9d3269e0b6f4caecd17d12eb9c768"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x08840bf78ffc1aebc8237d3c3d209f8337f0e0c2f975e5b1c3eac816d28d760e",
        "blockNumber": "0x8252ec",
        "from": "0x227f6757289a86c13eee2e91c2e6eb03f2ed11a6",
        "gas": "0xf4240",
        "gasPrice": "0x44f7347ed65",
        "hash": "0x05595f42e607777285b2e2e93f69ea5b20e9d3269e0b6f4caecd17d12eb9c768",
        "input": "0x38ed173900000000000000000000000000000000000000000367d92e7d6fba5cde0000000000000000000000000000000000000000000000000000000000000009fd101c00000000000000000000000000000000000000000000000000000000000000a0000000000000000000000000227f6757289a86c13eee2e91c2e6eb03f2ed11a6000000000000000000000000000000000000000000000000000000006473d2180000000000000000000000000000000000000000000000000000000000000003000000000000000000000000dd73dea10abc2bff99c60882ec5b2b81bb1dc5b20000000000000000000000002d03bece6747adc00e1a131bba1469c15fd11e03000000000000000000000000c21223249ca28397b4b6541dffaecc539bff0c59",
        "nonce": "0x30750",
        "r": "0x4fdf66fca692d396412e62f3e7664211b0722117ce30731c5628c2fa9a22ad0e",
        "s": "0x5723052482c4d3b4bfcb74742e4f24f04fa26f7480d1a432f17b47531bb1fa8e",
        "to": "0x145863eb42cf62847a6ca784e6416c1682b1b2ae",
        "transactionIndex": "0x2",
        "type": "0x0",
        "v": "0x56",
        "value": "0x0"
    }
}
```
