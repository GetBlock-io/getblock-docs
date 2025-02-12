---
description: >-
  Example code for the sendtoaddress  {disallowed} json-rpc method. Сomplete
  guide on how to use sendtoaddress  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# sendtoaddress {disallowed} - Dash

#### Parameters

`To Address` - string

A P2PKH or P2SH address to which the dash should be sent.

`Amount` - number (dash)

The amount to spent in dash.

`Comment` - string

Optional.

A locally-stored (not broadcast) comment assigned to this transaction. Default is no comment.

`Subtract Fee From Amount` - array

Optional.

An array of addresses. The fee will be equally divided by as many addresses as are entries in this array and subtracted from each address.

If this array is empty or not provided, the fee will be paid by the sender.

`Use InstantSend` - bool

Optional.

Deprecated and ignored since Dash Core 0.15.0

`Use CoinJoin` - bool

Optional.

If set to true, use CoinJoin funds only (default: false).

`conf_target` - number (int)

Optional.

Confirmation target (in blocks).

`estimate_mode` - string

Optional.

The fee estimate mode, must be one of: UNSET, ECONOMICAL, CONSERVATIVE. Default: UNSET

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "sendtoaddress",
"params": [null, null, null, [{"name": "Address", "type": "string (base58)", "description": ["Optional.", "An address previously listed as one of the recipients."], "value": null}], null, null, null, null],
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
