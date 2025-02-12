---
description: >-
  Example code for the web3_sha3 json-rpc method. Сomplete guide on how to use
  web3_sha3 json-rpc in GetBlock.io Web3 documentation.
---

# web3\_sha3 - Gnosis

#### Parameters

`DATA` - None

Data to convert to a SHA3 hash.

#### Request

```java
curl --location --request POST 'https://gno.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "web3_sha3",
"params": ["0x68656c6c6f20776f726c00"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x5e39a0a66544c0668bde22d61c47a8710000ece931f13b84d3b2feb44ec96d3f"
}
```
