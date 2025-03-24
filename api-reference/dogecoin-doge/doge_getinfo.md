---
description: >-
  Example code for the getinfo json-rpc method. Сomplete guide on how to use
  getinfo json-rpc in GetBlock.io Web3 documentation.
---

# getinfo - Dogecoin

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getinfo",
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
        "connections": 14,
        "difficulty": 4386136.832791463,
        "errors": "",
        "paytxfee": 1.0,
        "protocolversion": 70015,
        "proxy": "",
        "relayfee": 0.001,
        "testnet": false,
        "timeoffset": -1,
        "version": 1140400
    }
}
```
