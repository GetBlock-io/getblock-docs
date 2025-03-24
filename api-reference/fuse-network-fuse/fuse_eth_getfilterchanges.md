---
description: >-
  Example code for the eth_getFilterChanges json-rpc method. Сomplete guide on
  how to use eth_getFilterChanges json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getFilterChanges - Fuse Network

#### Parameters

`QUANTITY` - string

The filter id.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getFilterChanges",
"params": ["0x16"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": [
        "0x2fd6948de54641aa0f4d5ed2d197de893c54876f2545ca02bdac72543968d2af",
        "0x46e059e38707043aa44195a9ed597350ce9eaba5baddc3914eb616d1a573c43c",
        "0x1ef9f410b4f31ca45f45db1cc8ea2f00782d9108d9bb322c0c756928baddce0a",
        "0x320574a2a697d7332f220a92ed88e693796af672dd363b3196eec00a5eaebfa6"
    ]
}
```
