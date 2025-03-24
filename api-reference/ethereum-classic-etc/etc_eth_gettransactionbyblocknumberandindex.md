---
description: >-
  Example code for the eth_getTransactionByBlockNumberAndIndex json-rpc method.
  Сomplete guide on how to use eth_getTransactionByBlockNumberAndIndex json-rpc
  in GetBlock.io Web3 documentation.
---

# eth\_getTransactionByBlockNumberAndIndex - Ethereum Classic

#### Parameters

`QUANTITY|TAG` - string

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

`QUANTITY` - string

The transaction index position.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionByBlockNumberAndIndex",
"params": ["0xcdf167", "0x1"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0xbb50cb8d2d5698c1b10f1a845360ecf521ff18b08f71664a4639b6bdbb08fb38",
        "blockNumber": "0xcdf167",
        "chainId": "0x3d",
        "from": "0x0345f0eb7a8264a6d0c6498954128340d311ec04",
        "gas": "0x5208",
        "gasPrice": "0x4a817c800",
        "hash": "0xcdd15a0a55ce9a39a831057240191becbb9d39342e5773236d82afcc7923e08d",
        "input": "0x",
        "nonce": "0x0",
        "publicKey": "0x8fb41205f9ecfbbb242746bcfef49688548d45fc5206c61a3a965e309f944669882e8ccb22e4d8ba2d4c6644b4dc39756b7bb9783237be47d1f209e13c780519",
        "r": "0xa9d817452a5a474ce4280d0c3eb93a1e638f715fc89522bc07ec18ad333e0226",
        "raw": "0xf86d808504a817c8008252089469e6cffca250493510e10c939546d353a557e3e7880ddf59a2a553400080819da0a9d817452a5a474ce4280d0c3eb93a1e638f715fc89522bc07ec18ad333e0226a010289f85a77a2180a429b12e7316ef0be0a0858b4f70d518a5887a680fc349d7",
        "s": "0x10289f85a77a2180a429b12e7316ef0be0a0858b4f70d518a5887a680fc349d7",
        "to": "0x69e6cffca250493510e10c939546d353a557e3e7",
        "transactionIndex": "0x1",
        "v": "0x9d",
        "value": "0xddf59a2a5534000"
    }
}
```
