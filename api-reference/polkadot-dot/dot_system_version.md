---
description: >-
  Example code for the system_version json-rpc method. Сomplete guide on how to
  use system_version json-rpc in GetBlock.io Web3 documentation.
---

# system\_version - Polkadot

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://dot.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "system_version",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0.9.42-9b1fc27cec4"
}
```
