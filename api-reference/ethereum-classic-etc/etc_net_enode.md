---
description: >-
  Example code for the net_enode json-rpc method. Сomplete guide on how to use
  net_enode json-rpc in GetBlock.io Web3 documentation.
---

# net\_enode - Ethereum Classic

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "net_enode",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "enode://3d70e868399d6d9273bcc6e7fd0fba08e9c9bd3d133cf59d2820dcd891070f6b5bb6dbcf36fe4eca7c7b2ea39802134ff7859b4402ff52e5a8eb2ad879086ec8@127.0.0.1:30303"
}
```
