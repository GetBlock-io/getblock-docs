---
description: >-
  Example code for the getblockhashes json-rpc method. Сomplete guide on how to
  use getblockhashes json-rpc in GetBlock.io Web3 documentation.
---

# getblockhashes - Dash

#### Parameters

`Block Timestamp` - number (int)

The block timestamp for the newest block hash that should be returned.

`Block Timestamp` - number (int)

The block timestamp for the oldest block hash that should be returned.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getblockhashes",
"params": [1631178288, 1631176288],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": [
        "0000000010a16c6fbc6bd5cdc238c2beabcda334e97fde1500d59be4e6fc4b89",
        "000000009910885e811230c403e55aac6547d6df04ee671b2e8348524f73cab8",
        "000000004bbb3828db1c4d4491760336cec215087819ab656336f30d4095e3d2",
        "00000000ad2df2149aca2261a9a87c41e139dfe8f73d91db7ec0c1837fee21a0",
        "0000000074068a9e3a271d165da3deb28bc3f8c751dde97f460d8078d92a9d06"
    ]
}
```
