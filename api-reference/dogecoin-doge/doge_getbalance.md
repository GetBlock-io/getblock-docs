---
description: >-
  Example code for the getbalance  {disallowed} json-rpc method. Сomplete guide
  on how to use getbalance  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# getbalance {disallowed} - Dogecoin

#### Parameters

`account` - string

Optional.

If this parameter is specified, returns the balance in the account.

`minconf` - integer

Optional.

Minimum number of confirmations required for transferred balance.

#### Request

```java
curl --location --request POST 'https://doge.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getbalance",
"params": [null, 1],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": "0x11ffdea0d1ae800"
}
```
