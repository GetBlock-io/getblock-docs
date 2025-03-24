---
description: >-
  Example code for the getmininginfo json-rpc method. Сomplete guide on how to
  use getmininginfo json-rpc in GetBlock.io Web3 documentation.
---

# getmininginfo - Dogecoin

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
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
        "blocks": 3906176,
        "chain": "main",
        "currentblocksize": 0,
        "currentblocktx": 0,
        "currentblockweight": 0,
        "difficulty": 4386136.832791463,
        "errors": "",
        "networkhashps": 301649146285387.1,
        "pooledtx": 32
    }
}
```
