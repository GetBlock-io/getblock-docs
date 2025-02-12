---
description: >-
  Example code for the getblockchaininfo json-rpc method. Сomplete guide on how
  to use getblockchaininfo json-rpc in GetBlock.io Web3 documentation.
---

# getblockchaininfo - Dash

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getblockchaininfo",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "bestblockhash": "0000000000000019cd3a50eedc418372f7aa9060d6ded56a769a51a93dfe0e4a",
        "bip9_softforks": {
            "bip147": {
                "since": 939456,
                "startTime": 1524477600,
                "status": "active",
                "timeout": 1556013600
            },
            "csv": {
                "since": 622944,
                "startTime": 1486252800,
                "status": "active",
                "timeout": 1517788800
            },
            "dip0001": {
                "since": 782208,
                "startTime": 1508025600,
                "status": "active",
                "timeout": 1539561600
            },
            "dip0003": {
                "since": 1028160,
                "startTime": 1546300800,
                "status": "active",
                "timeout": 1577836800
            },
            "dip0008": {
                "since": 1088640,
                "startTime": 1557878400,
                "status": "active",
                "timeout": 1589500800
            },
            "dip0020": {
                "since": 1516032,
                "startTime": 1625097600,
                "status": "active",
                "timeout": 1656633600
            },
            "realloc": {
                "since": 1374912,
                "startTime": 1601510400,
                "status": "active",
                "timeout": 1633046400
            }
        },
        "blocks": 1535914,
        "chain": "main",
        "chainwork": "00000000000000000000000000000000000000000000615f0d845c321319b10d",
        "difficulty": 103704145.924301,
        "headers": 1535914,
        "initialblockdownload": false,
        "mediantime": 1631267277,
        "pruned": false,
        "size_on_disk": 26693764683,
        "softforks": [
            {
                "id": "bip34",
                "reject": {
                    "status": true
                },
                "version": 2
            },
            {
                "id": "bip66",
                "reject": {
                    "status": true
                },
                "version": 3
            },
            {
                "id": "bip65",
                "reject": {
                    "status": true
                },
                "version": 4
            }
        ],
        "verificationprogress": 0.9999984311220077,
        "warnings": ""
    }
}
```
