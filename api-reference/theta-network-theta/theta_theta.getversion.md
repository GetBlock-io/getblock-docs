---
description: >-
  Example code for the theta.GetVersion json-rpc method. Сomplete guide on how
  to use theta.GetVersion json-rpc in GetBlock.io Web3 documentation.
---

# theta.GetVersion - Theta Network

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "theta.GetVersion",
"params": {},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "git_hash": "dadc576ed7cc6a20f6ed4c174a3127e02737b933",
        "timestamp": "Sun 01 Aug 2021 05:29:47 PM UTC",
        "version": "3.1.0"
    }
}
```
