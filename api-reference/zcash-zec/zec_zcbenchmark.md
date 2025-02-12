---
description: >-
  Example code for the zcbenchmark  {disallowed} json-rpc method. Сomplete guide
  on how to use zcbenchmark  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# zcbenchmark {disallowed} - Zcash

#### Parameters

`benchmarktype` - string

type of benchmark

`samplecount` - string

count of samples to run.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "zcbenchmark",
"params": [null, null],
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
