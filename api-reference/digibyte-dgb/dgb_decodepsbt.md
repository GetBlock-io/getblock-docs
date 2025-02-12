---
description: >-
  Example code for the decodepsbt json-rpc method. Сomplete guide on how to use
  decodepsbt json-rpc in GetBlock.io Web3 documentation.
---

# decodepsbt - DigiByte

#### Parameters

`psbt` - string, required

#### Request

```java
curl --location --request POST 'https://dgb.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "decodepsbt",
"params": ["psbt"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": null
}
```
