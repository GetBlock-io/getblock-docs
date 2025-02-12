---
description: >-
  Example code for the plugins_reloadPluginConfig  {disallowed} json-rpc method.
  Сomplete guide on how to use plugins_reloadPluginConfig  {disallowed} json-rpc
  in GetBlock.io Web3 documentation.
---

# plugins\_reloadPluginConfig {disallowed} - Ethereum Classic

#### Parameters

`string` - string

Plugin

#### Request

```java
curl --location --request POST 'https://etc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "plugins_reloadPluginConfig",
"params": [null],
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
