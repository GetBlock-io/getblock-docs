---
description: >-
  Example code for the hmyv2_estimateGas json-rpc method. Сomplete guide on how
  to use hmyv2_estimateGas json-rpc in GetBlock.io Web3 documentation.
---

# hmyv2\_estimateGas - Harmony

#### Parameters

`object` - object

Transaction call object.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "hmyv2_estimateGas",
"params": [{"from": "0xFE3B557E8Fb62b89F4916B721be55cEb828dBd73", "to": "0x44Aa93095D6749A706051658B970b941c72c1D53", "value": "0x1"}],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x5208"
}
```
