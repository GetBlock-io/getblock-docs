---
description: >-
  Example code for the getspentinfo json-rpc method. Сomplete guide on how to
  use getspentinfo json-rpc in GetBlock.io Web3 documentation.
---

# getspentinfo - Dash

#### Parameters

`TXID` - string (hex)

The TXID of the transaction containing the relevant output, encoded as hex in RPC byte order.

`index` - number (int)

The block height to begin looking in.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getspentinfo",
"params": {"txid": "0456aaf51a8df21dd47c2a06ede046a5bf7403bcb95d14d1d71b178c189fb933", "index": 0},
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "txid": "14e874421350840e9d43965967c5a989e7d41ad361ef37484ee67d01d433ecfa",
        "index": 1,
        "height": 7742
    }
}
```
