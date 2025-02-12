---
description: >-
  Example code for the protx_update_registrar  {disallowed} json-rpc method.
  Сomplete guide on how to use protx_update_registrar  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# protx\_update\_registrar {disallowed} - Dash

#### Parameters

`submethod` - string

None

`proTxHash` - string (hex)

The hash of the provider transaction as hex in RPC byte order.

`operatorPubKey` - string (hex)

The operator public key. The private key does not have to be known. It has to match the private key which is later used when operating the masternode.

If set to an empty string, the currently active operator BLS public key is reused.

`votingAddress` - string (hex)

The voting address. The private key does not have to be known by your wallet. It has to match the private key which is later used when voting on proposals.

If set to an empty string, the currently active voting key address is reused.

`payoutAddress` - string (hex)

Optional.

The Dash address to use for masternode reward payments. If set to an empty string, the currently active payout address is reused.

`feeSourceAddress` - string

Optional.

If specified, the wallet will only use coins from this address to fund the ProTx. If not specified, payoutAddress will be used. The private key belonging to this address must be known in your wallet.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "protx",
"params": ["update_registrar", null, null, null, null, null],
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
