---
description: >-
  Example code for the getsmartcodeevent json-rpc method. Сomplete guide on how
  to use getsmartcodeevent json-rpc in GetBlock.io Web3 documentation.
---

# getsmartcodeevent - Ontology

#### Parameters

`height_or_hash` - string of integer

block height or transaction hash

`verbose` - integer

1 for verbose

#### Request

```java
curl --location --request POST 'https://ont.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getsmartcodeevent",
"params": ["db81c4e00f050c4f8e2f9c7e8201fddebd18458b3c48c73c18aa0532c7b5c43c", 1],
"id": "getblock.io"}'
```

#### Response

```java
{
    "desc": "SUCCESS",
    "error": 0,
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "CreatedContract": "0000000000000000000000000000000000000000",
        "GasConsumed": 50000000,
        "GasStepUsed": 20000,
        "Notify": [
            {
                "ContractAddress": "e5a49d7fd57e7178e189d3965d1ee64368a1036d",
                "IsEvm": false,
                "States": [
                    "7472616e73666572",
                    "64a222d3108f3dd8a896b7a73e71b40c1e75f113",
                    "d773cd246fd21f539530d00154ed1a8b527c9591",
                    "b804b16711"
                ]
            },
            {
                "ContractAddress": "e5a49d7fd57e7178e189d3965d1ee64368a1036d",
                "IsEvm": false,
                "States": [
                    "7472616e73666572",
                    "d773cd246fd21f539530d00154ed1a8b527c9591",
                    "1ff65884aff6058a402468972865a4be03b3202e",
                    "b804b16711"
                ]
            },
            {
                "ContractAddress": "0200000000000000000000000000000000000000",
                "IsEvm": false,
                "States": [
                    "transfer",
                    "AQwyT6CWUNn8yKVdXpvi7wwWtEJnqri7vW",
                    "AFmseVrdL9f9oyCzZefL9tG6UbviEH9ugK",
                    50000000
                ]
            }
        ],
        "State": 1,
        "TxHash": "db81c4e00f050c4f8e2f9c7e8201fddebd18458b3c48c73c18aa0532c7b5c43c",
        "TxIndex": 0
    }
}
```
