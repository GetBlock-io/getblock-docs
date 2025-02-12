---
description: >-
  Example code for the net_services json-rpc method. Сomplete guide on how to
  use net_services json-rpc in GetBlock.io Web3 documentation.
---

# net\_services - Ethereum Classic

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://etc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "net_services",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "jsonrpc": {
            "host": "0.0.0.0",
            "port": "8545"
        },
        "p2p": {
            "host": "127.0.0.1",
            "port": "30303"
        },
        "ws": {
            "host": "0.0.0.0",
            "port": "8546"
        }
    }
}
```
