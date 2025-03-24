---
description: >-
  Example code for the getchaintips json-rpc method. Сomplete guide on how to
  use getchaintips json-rpc in GetBlock.io Web3 documentation.
---

# getchaintips - DigiByte

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getchaintips",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": [
        {
            "branchlen": 0,
            "hash": "b034141fc8e8024752b39513c9ed6db14690bf3a7fe9911a89c83578a73f0457",
            "height": 13645713,
            "status": "active"
        },
        {
            "branchlen": 1,
            "hash": "c70cbb540bc9ae2fd14f72b4521176d49c8c28a457121879d3acca3a4ff5bec6",
            "height": 13645678,
            "status": "valid-fork"
        },
        {
            "branchlen": 1,
            "hash": "b2d7f758abd7cf0bc043f5fed9ded8889d02a0cfbc108ef42b89339900a5bee7",
            "height": 13645645,
            "status": "valid-headers"
        },
        {
            "branchlen": 1,
            "hash": "d0a6e1a82bbcdeedebf0b8ec5ba5ac61f60e689da79f6c494f51313c029cf12a",
            "height": 13645285,
            "status": "valid-headers"
        }
    ]
}
```
