---
description: >-
  Example code for the getmemoryinfo  {disallowed} json-rpc method. Сomplete
  guide on how to use getmemoryinfo  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# getmemoryinfo {disallowed} - Dash

#### Parameters

`mode` - string

Optional. Default = stats

Added in Dash Core 0.15.0

Determines what kind of information is returned.

\- stats returns general statistics about memory usage in the daemon.

\- mallocinfo returns an XML string describing low-level heap state (only available if compiled with glibc 2.10+).

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getmemoryinfo",
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
