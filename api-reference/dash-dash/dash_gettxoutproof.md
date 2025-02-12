---
description: >-
  Example code for the gettxoutproof json-rpc method. Сomplete guide on how to
  use gettxoutproof json-rpc in GetBlock.io Web3 documentation.
---

# gettxoutproof - Dash

#### Parameters

`TXID` - string (hex)

The TXID of the transaction containing the relevant output, encoded as hex in RPC byte order.

`header hash` - string

Optional.

If specified, looks for txid in the block with this hash.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "gettxoutproof",
"params": [["53fd7dcc171756ab56c1c9af76bdc398f6be69d97dcbe8696b53ff1b140c2249", "83dc6c8e03026c0317885f62a7072dfde10014967f59477a0f7b5fc52f44a784"], null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": "000000200744dc8495c586ddd11132b7204e248d592e1b10e37267082c00000000000000eddc7f557e52e111824d1100fed92779ffaa9c7f8a02c60cf6c04dfae80d0df130ce3961b3b42a19150bba42080000000484a7442fc55f7b0f7a47597f961400e1fd2d07a7625f8817036c02038e6cdc8349220c141bff536b69e8cb7dd969bef698c3bd76afc9c156ab561717cc7dfd5398f5ecaec6c8dbc0a1fb8f335f310cff00845f278a73d9423eadaed4529c54eba6c954ff561af9911cf06c838dc507d86834823946729e23ca504ede09d5e870011f"
}
```
