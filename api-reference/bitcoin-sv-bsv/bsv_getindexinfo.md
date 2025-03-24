---
description: >-
  Example code for the getindexinfo json-rpc method. Сomplete guide on how to
  use getindexinfo json-rpc in GetBlock.io Web3 documentation.
---

# getindexinfo - Bitcoin SV

#### Parameters

`index_name` - string, optional

Filter results for an index with a specific name.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getindexinfo",
"params": ["default"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {}
}
```
