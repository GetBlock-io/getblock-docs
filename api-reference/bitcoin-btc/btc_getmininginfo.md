---
description: >-
  Example code for the getmininginfo json-rpc method. Сomplete guide on how to
  use getmininginfo json-rpc in GetBlock.io Web3 documentation.
---

# getmininginfo - Bitcoin

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getmininginfo",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "blocks": 684635,
        "chain": "main",
        "currentblocksize": 1999960,
        "currentblocktx": 6269,
        "difficulty": 303698825886.4642,
        "networkhashps": 2.333876557488516e+18,
        "pooledtx": 6334,
        "warnings": "Warning: This version of Bitcoin Cash Node is old and may fall out of network consensus in 22 day(s). Please upgrade, or add expire=0 to your configuration file if you want to continue running this version. If you do nothing, the software will gracefully degrade by limiting its functionality in 22 day(s)."
    }
}
```
