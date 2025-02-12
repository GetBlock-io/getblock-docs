---
description: >-
  Example code for the eth_getTransactionByBlockHashAndIndex json-rpc method.
  Сomplete guide on how to use eth_getTransactionByBlockHashAndIndex json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_getTransactionByBlockHashAndIndex - Gnosis

#### Parameters

`DATA` - hex string

32-byte hash of a block.

`QUANTITY` - hex string

Integer representing the transaction index position.

#### Request

```java
curl --location --request POST 'https://gno.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionByBlockHashAndIndex",
"params": ["0xc11de1b67dd435ada2b83bbf0bb45cba5efab4e8dd00b100142e799ba902dddc", "0x0"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0xc11de1b67dd435ada2b83bbf0bb45cba5efab4e8dd00b100142e799ba902dddc",
        "blockNumber": "0x1ac46e3",
        "chainId": "0x64",
        "data": "0x095ea7b3000000000000000000000000fa5ed56a203466cbbc2430a43c66b9d8723528e70000000000000000000000000000000000000000000000018f907beb36ca2206",
        "from": "0x6c5c956c73e8379c001498551abcbb89426a27e5",
        "gas": "0xdb6c",
        "gasPrice": "0xbecb6240",
        "hash": "0xc6b20f02936f72709b65026d55f425cd7235f314b5e95a61bc21e54b2ef1fd49",
        "input": "0x095ea7b3000000000000000000000000fa5ed56a203466cbbc2430a43c66b9d8723528e70000000000000000000000000000000000000000000000018f907beb36ca2206",
        "maxFeePerGas": "0xbecb6241",
        "maxPriorityFeePerGas": "0xbecb6239",
        "nonce": "0xf",
        "r": "0xc57f98902d55fbe779bd7d981fc4214741a47b3c072d971b0361239e1a1e7b3b",
        "s": "0x47152598d3d796acd1cc943e987e60dc3405488ff5844bf8bf658cf36811e39d",
        "to": "0x4b1e2c2762667331bc91648052f646d1b0d35984",
        "transactionIndex": "0x0",
        "type": "0x2",
        "v": "0x0",
        "value": "0x0",
        "yParity": "0x0"
    }
}
```
