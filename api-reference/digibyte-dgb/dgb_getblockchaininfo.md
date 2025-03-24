---
description: >-
  Example code for the getblockchaininfo json-rpc method. Сomplete guide on how
  to use getblockchaininfo json-rpc in GetBlock.io Web3 documentation.
---

# getblockchaininfo - DigiByte

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
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
        "bestblockhash": "b034141fc8e8024752b39513c9ed6db14690bf3a7fe9911a89c83578a73f0457",
        "bip9_softforks": {
            "csv": {
                "since": 4394880,
                "startTime": 1489997089,
                "status": "active",
                "timeout": 1521891345
            },
            "nversionbips": {
                "since": 4394880,
                "startTime": 1489997089,
                "status": "active",
                "timeout": 1521891345
            },
            "odo": {
                "since": 9112320,
                "startTime": 1556668800,
                "status": "active",
                "timeout": 1588291200
            },
            "reservealgo": {
                "since": 8547840,
                "startTime": 1542672000,
                "status": "active",
                "timeout": 1574208000
            },
            "segwit": {
                "since": 4394880,
                "startTime": 1490355345,
                "status": "active",
                "timeout": 1521891345
            }
        },
        "blocks": 13645713,
        "chain": "main",
        "chainwork": "0000000000000000000000000000000000000000000c124085b05af5213ce5cc",
        "difficulties": {
            "odo": 758803.473162248,
            "qubit": 4838516.667810841,
            "scrypt": 111625.4745815573,
            "sha256d": 1778960458.9966,
            "skein": 8308860.184994861
        },
        "headers": 13645713,
        "initialblockdownload": false,
        "mediantime": 1631538179,
        "pruned": false,
        "size_on_disk": 22888984693,
        "softforks": [],
        "verificationprogress": 0.9999999300489301,
        "warnings": ""
    }
}
```
