---
description: >-
  Example code for the sendmany  {disallowed} json-rpc method. Сomplete guide on
  how to use sendmany  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# sendmany {disallowed} - Dash

#### Parameters

`Unused` - string

Deprecated: (previously account) will be removed in a later version of Dash Core

Must be set to "" for backwards compatibility.

`Outputs` - object

An object containing key/value pairs corresponding to the addresses and amounts to pay.

`Confirmations` - number (int)

Optional.

The minimum number of confirmations an incoming transaction must have for its outputs to be credited to this account's balance.

Outgoing transactions are always counted, as are move transactions made with the move RPC. If an account doesn't have a balance high enough to pay for this transaction, the payment will be rejected. Use 0 to spend unconfirmed incoming payments. Default is 1.

`addlocked` - bool

Optional.

If set to true, add the balance from InstantSend locked transactions.

If set to false (the default), InstantSend locked transaction balances are not included.

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
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "sendmany",
"params": [null, [{"name": "Address/Amount", "type": "string (base58) : number (dash)", "description": ["A key/value pair with a base58check-encoded string containing the P2PKH or P2SH address to pay as the key, and an amount of dash to pay as the value."], "value": null}], null, null, null, [{"name": "Address", "type": "string (base58)", "description": ["Optional.", "An address previously listed as one of the recipients."], "value": null}], null, null, null, null],
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
