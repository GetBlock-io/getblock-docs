---
description: >-
  Example code for the getmininginfo json-rpc method. Сomplete guide on how to
  use getmininginfo json-rpc in GetBlock.io Web3 documentation.
---

# getmininginfo - DigiByte

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://dgb.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getmininginfo",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "blocks": 13645716,
        "chain": "main",
        "currentblockweight": 4904,
        "difficulty": 111625.4745815573,
        "difficulty_odo": 758803.473162248,
        "difficulty_qubit": 4838516.667810841,
        "difficulty_scrypt": 111625.4745815573,
        "difficulty_sha256d": 1606081960.042681,
        "difficulty_skein": 8308860.184994861,
        "errors": "",
        "pooledtx": 0,
        "pow_algo": "scrypt",
        "pow_algo_id": 1,
        "warnings": ""
    }
}
```
