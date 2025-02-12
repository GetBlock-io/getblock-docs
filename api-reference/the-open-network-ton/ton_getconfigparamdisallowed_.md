---
description: >-
  Example code for the /getConfigParam  {disallowed} json-rpc method. Сomplete
  guide on how to use /getConfigParam  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# /getConfigParam {disallowed} - The Open Network (TON)

#### Parameters

`config_id` - query

required, integer

config id

`seqno` - query

optional, integer

Masterchain seqno. If not specified, latest blockchain state will be used.

#### Request

```java
curl --location --request GET 'https://ton.getblock.io/mainnet/getConfigParam' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json'
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
