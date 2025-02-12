---
description: >-
  Example code for the generatetodescriptor  {disallowed} json-rpc method.
  Сomplete guide on how to use generatetodescriptor  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# generatetodescriptor {disallowed} - Bitcoin

#### Parameters

`num_blocks` - numeric, required

How many blocks are generated immediately.

`maxtries` - numeric, optional, default=1000000

How many iterations to try.

`descriptor` - string, required

How many iterations to try.

#### Request

```java
curl --location --request POST 'https://btc.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "generatetodescriptor",
"params": [null, null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "result": null,
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```
