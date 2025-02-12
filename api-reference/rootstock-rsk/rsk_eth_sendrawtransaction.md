---
description: >-
  Example code for the eth_sendRawTransaction json-rpc method. Сomplete guide on
  how to use eth_sendRawTransaction json-rpc in GetBlock.io Web3 documentation.
---

# eth\_sendRawTransaction - Rootstock

#### Parameters

`DATA` - string

The signed transaction data.

#### Request

```java
wscat -c wss://rsk.getblock.io/YOUR-API-KEY/mainnet/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_sendRawTransaction",
"params": ["0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"],
"id": "getblock.io"}
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
}
```
