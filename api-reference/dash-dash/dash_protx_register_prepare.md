---
description: >-
  Example code for the protx_register_prepare  {disallowed} json-rpc method.
  Сomplete guide on how to use protx_register_prepare  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# protx\_register\_prepare {disallowed} - Dash

#### Parameters

`submethod` - string

None

`collateralHash` - string (hex)

The collateral transaction hash.

`collateralIndex` - string (hex)

The collateral transaction output index.

`ipAndPort` - string

IP and port in the form IP:PORT.

Must be unique on the network. Can be set to 0, which will require a ProUpServTx afterwards.

`ownerAddress` - string (hex)

The Dash address to use for payee updates and proposal voting. The corresponding private key does not have to be known by this wallet.

The address must be unused and must differ from the collateralAddress.

`operatorPubKey` - string (hex)

The operator public key. The private key does not have to be known.

It has to match the private key which is later used when operating the masternode.

`votingAddress` - string (hex)

The voting address. The private key does not have to be known by your wallet. It has to match the private key which is later used when voting on proposals.

If set to an empty string, ownerAddress will be used.

`operatorReward` - number

The fraction in % to share with the operator. The value must be between 0.00 and 100.00.

Note: If non-zero, ipAndPort must be zero as well.

`payoutAddress` - string

The Dash address to use for masternode reward payments.

`feeSourceAddress` - string

Optional.

If specified, the wallet will only use coins from this address to fund the ProTx. If not specified, payoutAddress will be used. The private key belonging to this address must be known in your wallet.

`submit` - boolean

Optional.

If true (default), the resulting transaction is sent to the network.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "protx",
"params": ["register_prepare", null, null, null, null, null, null, null, null, null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```
