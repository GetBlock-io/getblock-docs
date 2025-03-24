---
description: >-
  Example code for the quorum_dkgstatus  {disallowed} json-rpc method. Сomplete
  guide on how to use quorum_dkgstatus  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# quorum\_dkgstatus {disallowed} - Dash

#### Parameters

`submethod` - string

None

`detail_level` - number

Optional.

Detail level of output (default: 0):

\- 0 - Only show counts (default)

\- 1 - Show member indexes

\- 2 - Show member's ProTxHashes

Note: Works only when Spork 17 is enabled and only displays details related to the node running the command.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "quorum",
"params": ["dkgstatus", null],
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
