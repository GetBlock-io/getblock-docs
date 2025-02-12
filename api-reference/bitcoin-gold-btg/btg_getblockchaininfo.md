---
description: >-
  Example code for the getblockchaininfo json-rpc method. Сomplete guide on how
  to use getblockchaininfo json-rpc in GetBlock.io Web3 documentation.
---

# getblockchaininfo - Bitcoin Gold

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://btg.getblock.io/mainnet' 
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
        "bestblockhash": "000000007cc1fc836d0992202730ca71f77a8cf2a35974ca300dadbf8bc1e090",
        "bip9_softforks": {
            "csv": {
                "since": 419328,
                "startTime": 1462060800,
                "status": "active",
                "timeout": 1493596800
            },
            "segwit": {
                "since": 481824,
                "startTime": 1479168000,
                "status": "active",
                "timeout": 1510704000
            }
        },
        "blocks": 702634,
        "chain": "main",
        "chainwork": "000000000000000000000000000000000000000000a21782bdfc4d22791bf052",
        "difficulty": 197682.9725910754,
        "headers": 702634,
        "initialblockdownload": false,
        "mediantime": 1631008651,
        "pruned": false,
        "size_on_disk": 163410304062,
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
        "verificationprogress": 0.9999999981038389,
        "warnings": ""
    }
}
```
