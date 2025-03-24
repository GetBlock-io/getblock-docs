---
description: >-
  Example code for the listunspent  {disallowed} json-rpc method. Сomplete guide
  on how to use listunspent  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# listunspent {disallowed} - Dash

#### Parameters

`Minimum Confirmations` - number (int)

Optional.

The minimum number of confirmations the transaction containing an output must have in order to be returned. Use 0 to return outputs from unconfirmed transactions. Default is 1.

`Maximum Confirmations` - number (int)

Optional.

The maximum number of confirmations the transaction containing an output may have in order to be returned. Default is 9999999 (\~10 million).

`Addresses` - array

Optional.

If present, only outputs which pay an address in this array will be returned.

`Include Unsafe` - bool

Optional.

Include outputs that are not safe to spend . See description of safe attribute below. Default is true.

`Query Options` - json

Optional.

JSON with query options. Available options: - minimumAmount: Minimum value of each UTXO in DASH - maximumAmount: Maximum value of each UTXO in DASH - maximumCount: Maximum number of UTXOs - minimumSumAmount: Minimum sum value of all UTXOs in DASH - cointType: Filter coinTypes as follows: - - 0 = ALL\_COINS, - - 1 = ONLY\_FULLY\_MIXED, - - 2 = ONLY\_READY\_TO\_MIX, - - 3 = ONLY\_NONDENOMINATED, - - 4 = ONLY\_MASTERNODE\_COLLATERAL, - - 5 = ONLY\_COINJOIN\_COLLATERAL

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "None",
"params": [null, null, [{"name": "address", "type": "string (base58)", "description": ["A P2PKH or P2SH address"], "value": null}], null, null],
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
