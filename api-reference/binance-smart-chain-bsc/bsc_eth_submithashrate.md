---
description: >-
  Example code for the eth_submitHashrate  {disallowed} json-rpc method.
  Сomplete guide on how to use eth_submitHashrate  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_submitHashrate {disallowed} - Binance Smart Chain

#### Parameters

`DATA, 32 Bytes` - hex string

Hexadecimal string representation of the hash rate.

`DATA, 32 Bytes` - hex string

Random hexadecimal ID identifying the client.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_submitHashrate",
"params": ["0x0000000000000000000000000000000000000000000000000000000000500000", "0x59daa26581d0acd1fce254fb7e85952f4c09d0915afd33d3886cd914bc7d283c"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```
