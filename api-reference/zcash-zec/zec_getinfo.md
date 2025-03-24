---
description: >-
  Example code for the getinfo json-rpc method. Сomplete guide on how to use
  getinfo json-rpc in GetBlock.io Web3 documentation.
---

# getinfo - Zcash

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
        "balance": 0.0,
        "blocks": 1384346,
        "build": "v4.4.1",
        "connections": 8,
        "difficulty": 47115695.15933816,
        "errors": "",
        "errorstimestamp": 1631190613,
        "keypoololdest": 1625167933,
        "keypoolsize": 101,
        "paytxfee": 0.0,
        "protocolversion": 170013,
        "proxy": "",
        "relayfee": 1e-06,
        "subversion": "/MagicBean:4.4.1/",
        "testnet": false,
        "timeoffset": 0,
        "version": 4040150,
        "walletversion": 60000
    }
}
```
