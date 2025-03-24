---
description: >-
  Example code for the gettxout json-rpc method. Сomplete guide on how to use
  gettxout json-rpc in GetBlock.io Web3 documentation.
---

# gettxout - Dash

#### Parameters

`TXID` - string (hex)

The TXID of the transaction containing the relevant output, encoded as hex in RPC byte order.

`vout` - number (int)

The output index number (vout) of the output within the transaction; the first output in a transaction is vout 0.

`Unconfirmed` - boolean

Optional.

Set to true to display unconfirmed outputs from the memory pool; set to false (the default) to only display outputs from confirmed transactions.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "gettxout",
"params": ["83dc6c8e03026c0317885f62a7072dfde10014967f59477a0f7b5fc52f44a784", 0, false],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "bestblock": "0000000000000019cd3a50eedc418372f7aa9060d6ded56a769a51a93dfe0e4a",
        "coinbase": true,
        "confirmations": 570,
        "scriptPubKey": {
            "addresses": [
                "XmzfivrzYQ7B7oBMZKwPRdhjB1iNvX71XZ"
            ],
            "asm": "OP_DUP OP_HASH160 7c086eada12bdb10a265c16c08a7ae87366bd481 OP_EQUALVERIFY OP_CHECKSIG",
            "hex": "76a9147c086eada12bdb10a265c16c08a7ae87366bd48188ac",
            "reqSigs": 1,
            "type": "pubkeyhash"
        },
        "value": 1.23223328
    }
}
```
