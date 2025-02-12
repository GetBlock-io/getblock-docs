---
description: >-
  Example code for the dag_getHeads json-rpc method. Сomplete guide on how to
  use dag_getHeads json-rpc in GetBlock.io Web3 documentation.
---

# dag\_getHeads - Fantom

#### Parameters

`DATA` - None

Epoch number (hex), “latest” or “pending” to specify epoch.

#### Request

```java
curl --location --request POST 'https://ftm.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "dag_getHeads",
"params": ["pending"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": [
        "0x00034398000009f17dd5914154b39da1cc474cae0bb2f53b9ebdf1ea21f6b39d",
        "0x00034398000009f1cea4871c92bd162d214d548cea99a599aff688b16cbe2f15",
        "0x00034398000009f0feb54bc15f7cbfafec3c1b6de97379626b112002266b50a4"
    ]
}
```
