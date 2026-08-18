---
description: >-
  Example code for the quorum_list  {disallowed} json-rpc method. Сomplete guide
  on how to use quorum_list  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# quorum\_list {disallowed} - Dash

#### Parameters

`submethod` - string

None

`count` - nnumber

Optional.

Number of quorums to list. Will list active quorums if count is not specified.

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "quorum",
"params": ["list", null],
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
