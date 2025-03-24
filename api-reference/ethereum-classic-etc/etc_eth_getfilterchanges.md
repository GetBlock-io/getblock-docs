---
description: >-
  Example code for the eth_getFilterChanges json-rpc method. Сomplete guide on
  how to use eth_getFilterChanges json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getFilterChanges - Ethereum Classic

#### Parameters

`data` - string

Filter ID.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getFilterChanges",
"params": ["0x729eda23823d3ffeb7cd58fb7f2aadda"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": [
        "0xda2bfe44bf85394f0d6aa702b5af89ae50ae22c0928c18b8903d9269abe17e0b",
        "0x88cd3a37306db1306f01f7a0e5b25a9df52719ad2f87b0f88ee0e6753ed4a812",
        "0x4d4c731fe129ff32b425e6060d433d3fde278b565bbd1fd624d5a804a34f8786"
    ]
}
```
