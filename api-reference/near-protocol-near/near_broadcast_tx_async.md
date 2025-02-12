---
description: >-
  Example code for the broadcast_tx_async json-rpc method. Сomplete guide on how
  to use broadcast_tx_async json-rpc in GetBlock.io Web3 documentation.
---

# broadcast\_tx\_async - NEAR Protocol

#### Parameters

`SignedTransaction` - string

signed transaction encoded in base64.

#### Request

```java
curl --location --request POST 'https://near.getblock.io/mainnet' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "broadcast_tx_async",
"params": ["DgAAAHNlbmRlci50ZXN0bmV0AOrmAai64SZOv9e/naX4W15pJx0GAap35wTT1T/DwcbbDwAAAAAAAAAQAAAAcmVjZWl2ZXIudGVzdG5ldNMnL7URB1cxPOu3G8jTqlEwlcasagIbKlAJlF5ywVFLAQAAAAMAAACh7czOG8LTAAAAAAAAAGQcOG03xVSFQFjoagOb4NBBqWhERnnz45LY4+52JgZhm1iQKz7qAdPByrGFDQhQ2Mfga8RlbysuQ8D8LlA6bQE="],
"id": "getblock.io"}'
```

#### Response

```java
{
    "jsonrpc": "2.0",
    "result": "6zgh2u9DqHHiXzdy9ouTP7oGky2T4nugqzqt9wJZwNFm",
    "id": "getblock.io"
}
```
