---
description: >-
  Example code for the getblocktemplate json-rpc method. Сomplete guide on how
  to use getblocktemplate json-rpc in GetBlock.io Web3 documentation.
---

# getblocktemplate - DigiByte

#### Parameters

`template_request` - json object, optional, default={}

Format of the template

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getblocktemplate",
"params": [null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "bits": "1b0096d0",
        "capabilities": [
            "proposal"
        ],
        "coinbaseaux": {
            "flags": ""
        },
        "coinbasevalue": 49718329627,
        "curtime": 1631538249,
        "height": 13645716,
        "longpollid": "000000000000000101374f4fe466e1ac44c78e73f540ea11bf54d89133e70e2c2034210",
        "mintime": 1631538188,
        "mutable": [
            "time",
            "transactions",
            "prevblock"
        ],
        "noncerange": "00000000ffffffff",
        "previousblockhash": "000000000000000101374f4fe466e1ac44c78e73f540ea11bf54d89133e70e2c",
        "rules": [
            "csv",
            "segwit",
            "nversionbips",
            "reservealgo",
            "odo"
        ],
        "sigoplimit": 80000,
        "sizelimit": 4000000,
        "target": "00000000000096d0000000000000000000000000000000000000000000000000",
        "transactions": [
            {
                "data": "02000000014a04b8e44d892b20e580434ac732958445ffbdfa62b252c54c1335f1e2747f0e010000006b483045022100b952db98b1a02705230ef15b9013c577a1e1362948f24c0068968f9dae4b9c1802206941ce772e701b32861ee7bdcd52a189afa90a838ecd06c1852ea427b797e1f2012103a4325d8098013d75a0707d1ffb65c999e7c07007aaf82b5d43c3573e659fadadfdffffff0200325f9d080000001976a914b809b072d2c18423b29214f758f4da17a383589388ac90b1f865000000001976a914a7fd0e09823ffc3a0eb4af2e245d01121957509088ac00000000",
                "depends": [],
                "fee": 67600,
                "hash": "c7779710ae0c4d7e6d1f1f6a8b71970cae831a3c8203c9fa04cfb7fd1efb00dd",
                "sigops": 8,
                "txid": "c7779710ae0c4d7e6d1f1f6a8b71970cae831a3c8203c9fa04cfb7fd1efb00dd",
                "weight": 904
            }
        ],
        "vbavailable": {},
        "vbrequired": 0,
        "version": 536870914,
        "weightlimit": 4000000
    }
}
```
