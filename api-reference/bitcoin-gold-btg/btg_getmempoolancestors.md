---
description: >-
  Example code for the getmempoolancestors json-rpc method. Сomplete guide on
  how to use getmempoolancestors json-rpc in GetBlock.io Web3 documentation.
---

# getmempoolancestors - Bitcoin Gold

#### Parameters

`txid` - string, required

The transaction id (must be in mempool)

`verbose` - boolean, optional, default=false

True for a json object, false for array of transaction ids

#### Request

```java
curl --location --request POST 'https://btg.getblock.io/mainnet' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getmempoolancestors",
"params": ["a4aef4dd6721ff3e39a6f9b55d87ec2b72bc5bb55ea806ba75aa6c27b2a335df", null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": [
        "09338b612e0dbe246dc5f60b30e83029fd2d4ce84272afcce902215c9832de1f",
        "133e7651074cc4559bcab749c389fcfe6de9c41179a6a4efdb2ddde31be80023",
        "12018e5f38a691d567e579d0a89be6fce59ebdcbc05c96c90eff3c3fae5bf225",
        "bca8ecd71153a9857aba8cefe57272dd8f6d8a166bbca23ff9458e7df3b43127",
        "67f267952ace1b1dbd2ffe1c5b0b130590fe9c46dba02dfadf03c94b6c5a1529"
    ]
}
```
