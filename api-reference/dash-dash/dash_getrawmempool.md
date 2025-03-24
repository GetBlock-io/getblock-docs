---
description: >-
  Example code for the getrawmempool json-rpc method. Сomplete guide on how to
  use getrawmempool json-rpc in GetBlock.io Web3 documentation.
---

# getrawmempool - Dash

#### Parameters

`format` - boolean

Optional.

Set to true to get verbose output describing each transaction in the memory pool; set to false (the default) to only get an array of TXIDs for transactions in the memory pool.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getrawmempool",
"params": [false],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": [
        "2e61113910ab9a3a7730298b571efcadcffd4155e46125a9cb91589c046901b9",
        "88cd24bdee7d901c56a730a4d70a7ff3a5d4f26a448134d52149dd7058f924cf",
        "fd3fd4df2e1732adb0a67cff07babd5638ef21e2d21808dbef7b7b3b9e99cb69",
        "9f0d976dd36bc43c4700b5b74f51185767a340086883bf9bfea5d27343530fe0",
        "a217e6d9bf24a38b1489e997b8d9ac324b4227d10b5939e9a893de1ce2bd0fec",
        "8a57b01925276252aa7fb66aad31d85c8d8e24f27b14fa51eb065424b0e1df55",
        "f02a5ebf04a4567d71b30cc90ebfb399471828b28dda0a69891b4a57eeda733b",
        "2243ac2ae1273de4275157c1607261f939374e323339512d0ff8f7408fe4309b",
        "ae7069fc93af340c5a40191bac5104ba7dbe241dbeda118e7054ee7d81ab47a9",
        "26eecf348dd837c76e54d74a85835d09dfbaa2fe827087b97e963bbbd369b5a7",
        "cff46d577d7b3dd9498600553e8898e03327d5183f6064f9c83b12739bd124c7",
        "d0b89684b7fc059dfff077d03bca0ac0ef5467a6561480e8049aa422e8a4d4bf"
    ]
}
```
