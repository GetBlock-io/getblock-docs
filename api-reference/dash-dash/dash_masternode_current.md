---
description: >-
  Example code for the masternode_current  {disallowed} json-rpc method.
  Сomplete guide on how to use masternode_current  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# masternode\_current {disallowed} - Dash

#### Parameters

`method name` - string

None

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "masternode",
"params": ["current"],
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
