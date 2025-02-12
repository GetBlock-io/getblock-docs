---
description: >-
  Example code for the quorum_selectquorum  {disallowed} json-rpc method.
  Сomplete guide on how to use quorum_selectquorum  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# quorum\_selectquorum {disallowed} - Dash

#### Parameters

`submethod` - string

None

`llmqType` - number

Type of quorums to list:

\- 1 - LLMQ\_50\_60

\- 2 - LLMQ\_400\_60

\- 3 - LLMQ\_400\_85

\- 4 - LLMQ\_100\_67

`id` - string (hex)

Signing request ID

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "quorum",
"params": ["selectquorum", null, null],
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
