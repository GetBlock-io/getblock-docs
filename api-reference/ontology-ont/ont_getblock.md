---
description: >-
  Example code for the getblock json-rpc method. Сomplete guide on how to use
  getblock json-rpc in GetBlock.io Web3 documentation.
---

# getblock - Ontology

#### Parameters

`hash_or_height` - string or integer

block hash or height.

`verbose` - integer

1 for verbose response, 0 for raw response

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getblock",
"params": ["b4553dd4a5b3255aa78d20b2f2f0ec9d3a52d6ef2c99bfa23fd1f67001d9dd8b", 1],
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
        "Hash": "b4553dd4a5b3255aa78d20b2f2f0ec9d3a52d6ef2c99bfa23fd1f67001d9dd8b",
        "Header": {
            "BlockRoot": "5fe1ca66deeaf5ab2f1cc49cf1e120f96aa3292164325d3c2945e09d82e971f1",
            "Bookkeepers": [
                "0251f06bc247b1da94ec7d9fe25f5f913cedaecba8524140353b826cf9b1cbd9f4",
                "02bcdd278a27e4969d48de95d6b7b086b65b8d1d4ff6509e7a9eab364a76115af7",
                "02765d98bb092962734e365bd436bdc80c5b5991dcf22b28dbb02d3b3cf74d6444"
            ],
            "ConsensusData": 17295462332127119810,
            "ConsensusPayload": "7b226c6561646572223a31342c227672665f76616c7565223a22424277636e673330784632436f6361475a6e58684a625a6a4c354f5356305359694d4338306a6d50526c73757645794855345858744168434363375a71586e64477a595a70304e6b4d5a6a554241354977525a555a43553d222c227672665f70726f6f66223a224b6e50384a41596b326d506d384e6c6f52653135715a555876467766357734363079474f43624374563334454a4c706c514d4e72487671474f53566f7030616e4546434e344f4b4c644b47575863714533636a664f673d3d222c226c6173745f636f6e6669675f626c6f636b5f6e756d223a31363139343330302c226e65775f636861696e5f636f6e666967223a6e756c6c7d",
            "Hash": "b4553dd4a5b3255aa78d20b2f2f0ec9d3a52d6ef2c99bfa23fd1f67001d9dd8b",
            "Height": 16224151,
            "NextBookkeeper": "AFmseVrdL9f9oyCzZefL9tG6UbvhPbdYzM",
            "PrevBlockHash": "95bb5877ddc97dcdd49fa26597528885755daf727ddd46cfc67c624824a4e4b9",
            "SigData": [
                "ea9c976a7e3a529217f9294fc90308176b0954710fd202cf87a72fdccea08828691ccb609c5d3770ac3437d68934e4e645d5278c9289ad1be8f70df94298cfc2",
                "16ca6ac8e09102386e00c375750c78236ff0acc2bd18a4dcc01728bf6260bd3907b867514b34495f718f4d2a7d506c056a058e0539234afe8789b919f1d7cd26",
                "bed6708dbc1003dcfe909071b2781bcf2bb35c22683df564f7e7f8cce08cd92e3ae088b6dece1abc2e862d939fd1ca5351d20ef3033cb7a5e0aab70268dd7da0",
                "9143e52c2dd2e4825983c0e930b30f8bc4d1aa71d570b3c0ce5dcf3baa45ddd24183b24cbe97f3ff628955aa3e1e9e2f81b3b24166edee6c7cbcee66d03513ac"
            ],
            "Timestamp": 1686828063,
            "TransactionsRoot": "0000000000000000000000000000000000000000000000000000000000000000",
            "Version": 0
        },
        "Size": 1708,
        "Transactions": []
    }
}
```
