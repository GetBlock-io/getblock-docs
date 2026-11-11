---
description: >-
  Example code for the getmininginfo json-rpc method. Сomplete guide on how to
  use getmininginfo json-rpc in GetBlock.io Web3 documentation.
---

# getmininginfo - Bitcoin Gold

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
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
        "blocks": 702634,
        "chain": "main",
        "currentblocktx": 0,
        "currentblockweight": 4000,
        "difficulty": 197682.9725910754,
        "networkhashps": 2324661.797680737,
        "pooledtx": 0,
        "warnings": ""
    }
}
```
