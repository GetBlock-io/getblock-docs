---
description: >-
  Example code for the system_properties json-rpc method. Сomplete guide on how
  to use system_properties json-rpc in GetBlock.io Web3 documentation.
---

# system\_properties - Moonriver

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "system_properties",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "ss58Format": 0,
        "tokenDecimals": 10,
        "tokenSymbol": "movr"
    }
}
```
