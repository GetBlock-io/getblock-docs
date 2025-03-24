---
description: >-
  Example code for the get_version json-rpc method. Сomplete guide on how to use
  get_version json-rpc in GetBlock.io Web3 documentation.
---

# get\_version - Monero

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "get_version",
"params": {},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "release": false,
        "status": "OK",
        "untrusted": false,
        "version": 196613
    }
}
```
