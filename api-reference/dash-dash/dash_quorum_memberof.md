---
description: >-
  Example code for the quorum_memberof  {disallowed} json-rpc method. Сomplete
  guide on how to use quorum_memberof  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# quorum\_memberof {disallowed} - Dash

#### Parameters

`submethod` - string

None

`proTxHash` - string

ProTxHash of the masternode.

`scanQuorumsCount` - number

Optional.

Number of quorums to scan for. If not specified, the active quorum count for each specific quorum type is used.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "quorum",
"params": ["memberof", null, null],
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
