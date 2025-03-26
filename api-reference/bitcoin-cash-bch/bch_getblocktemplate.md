---
description: >-
  Example code for the getblocktemplate json-rpc method. Сomplete guide on how
  to use getblocktemplate json-rpc in GetBlock.io Web3 documentation.
---

# getblocktemplate - Bitcoin Cash

#### Parameters

`template_request` - json object, optional, default={}

Format of the template

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
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
        "bits": "1803a597",
        "capabilities": [
            "proposal"
        ],
        "coinbaseaux": {
            "flags": ""
        },
        "coinbasevalue": 628148220,
        "curtime": 1619171227,
        "height": 684636,
        "longpollid": "00000000000000000192b1487a01331fe09ef2c7ee09a0f2e36f049eb6ada5a864791970",
        "mintime": 1619163815,
        "mutable": [
            "time",
            "transactions",
            "prevblock"
        ],
        "noncerange": "00000000ffffffff",
        "previousblockhash": "00000000000000000192b1487a01331fe09ef2c7ee09a0f2e36f049eb6ada5a8",
        "sigoplimit": 226950,
        "sizelimit": 32000000,
        "target": "000000000000000003a597000000000000000000000000000000000000000000",
        "transactions": [
            {
                "data": "020000000168f1dc79ec68a2ac46669106f6df2b8af1109a592362498ac60d19b7a2012a80010000006a4730440220278e62223666d402c4bf6cb676d104a889591387e4617086042cf790f98483f402207c4a945a8da13dd62577942fb42cd9746961398efc89e0528a0649664812ad254121031b3c18ce8efcf9a1ce214cb5c62489e842663b47908b43f3af789d840af71efbffffffff0270ec1b00000000001976a914d54ef84280ff5d254ea038c170f4cee78d8d77c688acf066c100000000001976a914842b152a0bbd4647afaeceec8a6afaa90668e7c788ac00000000",
                "fee": 10000,
                "hash": "00141468b749e92a243a3653e214fa152b44ff8611b96f2927b84f7114eb4ae8",
                "sigops": 1,
                "txid": "00141468b749e92a243a3653e214fa152b44ff8611b96f2927b84f7114eb4ae8"
            }
        ],
        "version": 536870912
    }
}
```
