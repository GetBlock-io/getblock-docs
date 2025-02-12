---
description: >-
  Example code for the eth_getTransactionByBlockNumberAndIndex json-rpc method.
  Сomplete guide on how to use eth_getTransactionByBlockNumberAndIndex json-rpc
  in GetBlock.io Web3 documentation.
---

# eth\_getTransactionByBlockNumberAndIndex - Cronos

#### Parameters

`QUANTITY|TAG` - hex string

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

`QUANTITY` - hex string

The transaction index position.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/'  
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionByBlockNumberAndIndex",
"params": ["0x8252EC", "0x0"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "accessList": [],
        "blockHash": "0x08840bf78ffc1aebc8237d3c3d209f8337f0e0c2f975e5b1c3eac816d28d760e",
        "blockNumber": "0x8252ec",
        "chainId": "0x19",
        "from": "0x8a4f3a4ed9af1a85d9e455dd8d1222507f9f3702",
        "gas": "0x6c22f",
        "gasPrice": "0x44fccac8840",
        "hash": "0xc7c1d08c38bb7f0606d4e4f526f38bdd62dc1cb42a677bcb9ffea76f2f15ad59",
        "input": "0xe2bbb1580000000000000000000000000000000000000000000000000000000000000003000000000000000000000000000000000000000000000008619b51cb1b96f5cf",
        "maxFeePerGas": "0x5d201d51bf7",
        "maxPriorityFeePerGas": "0x59682f00",
        "nonce": "0x2fb",
        "r": "0x9f7fa9538baab5006bbe050b1f7fafb34c2ee9d032e520968e71e16515613633",
        "s": "0x647cf298a25d6d4a4a285565b5f82cc3e462746c3229ca964597f15e87206af7",
        "to": "0xab50fb1117778f293cc33ac044b5579fb03029d0",
        "transactionIndex": "0x0",
        "type": "0x2",
        "v": "0x0",
        "value": "0x0"
    }
}
```
