---
description: >-
  Example code for the thetacli.Send json-rpc method. Сomplete guide on how to
  use thetacli.Send json-rpc in GetBlock.io Web3 documentation.
---

# thetacli.Send - Theta Network

#### Parameters

`chain_id` - string

ID of the chain.

`from` - string

The address of the account to send tokens from.

`to` - string

The address of the receipient account.

`thetawei` - string

The amount of Theta tokens to be sent (in TFuelWei, 1 TFuel = 10^18 TFuelWei).

`tfuelwei` - string

The amount of TFuel tokens to be sent (in TFuelWei, 1 TFuel = 10^18 TFuelWei).

`fee` - string

The transaction fee in TFuelWei.

`sequence` - string

The expected sequence number of the from account.

`async` - boolean

If async is set to false, the RPC call will wait until the transaction has been included in a block, or a timeout reached. Otherwise, the RPC call will return immediately with the transaction hash.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "thetacli.Send",
"params": {"chain_id": "privatenet", "from": "0x2E833968E5bB786Ae419c4d13189fB081Cc43bab", "to": "0xA47B89c94a50C32CEACE9cF64340C4Dce6E5EcC6", "thetawei": "99000000000000000000", "tfuelwei": "88000000000000000000", "fee": "1000000000000", "sequence": "6", "async": true},
"id": "getblock.io"}'
```

#### Response

```java
{
    "result": {
        "hash": "0xe3e82ae1e08ca49f85842729bd3c70ba0874d59cca3812fe0807506463851d22",
        "block": null
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
