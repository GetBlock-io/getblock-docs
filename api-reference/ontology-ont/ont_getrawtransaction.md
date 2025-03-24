---
description: >-
  Example code for the getrawtransaction json-rpc method. Сomplete guide on how
  to use getrawtransaction json-rpc in GetBlock.io Web3 documentation.
---

# getrawtransaction - Ontology

#### Parameters

`hash` - string

transaction hash

`verbose` - integer

optional

1 for verbose response

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getrawtransaction",
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
        "Attributes": [],
        "GasLimit": 3924288,
        "GasPrice": 2500,
        "Hash": "db81c4e00f050c4f8e2f9c7e8201fddebd18458b3c48c73c18aa0532c7b5c43c",
        "Height": 16224161,
        "Nonce": 3937122241,
        "Payer": "AQwyT6CWUNn8yKVdXpvi7wwWtEJnqri7vW",
        "Payload": {
            "Code": "05b804b16711141ff65884aff6058a402468972865a4be03b3202e14d773cd246fd21f539530d00154ed1a8b527c959153c105b804b1671114d773cd246fd21f539530d00154ed1a8b527c95911464a222d3108f3dd8a896b7a73e71b40c1e75f11353c152c10d7472616e736665724d756c7469676d03a16843e61e5d96d389e178717ed57f9da4e5"
        },
        "Sigs": [
            {
                "M": 1,
                "PubKeys": [
                    "038720cec04db3a5ffec5424e33e8920d75d6a39156bacef374849d8bbfb8bcf85"
                ],
                "SigData": [
                    "012c44bc728de1806d320de54d026d8f65e84fae6fd0d2a93455fa27dc989db8741158185205449339984e42ae0ee2731fc9934c2478ff72a1036eb551d105f766"
                ]
            },
            {
                "M": 1,
                "PubKeys": [
                    "032db262ce294bf3889241995cb4682014e1b2ab89b3a1a8f3353b038c3a60b401"
                ],
                "SigData": [
                    "018458713b3e62401e06bd380456c63adb97bd8f4ce6dc3e85d21ede63e69dd8631cc2d621aae4bc7a742e165e46a69acc526196a0188fe9f5ddcf6b1905ff63c9"
                ]
            }
        ],
        "TxType": 209,
        "Version": 0
    }
}
```
