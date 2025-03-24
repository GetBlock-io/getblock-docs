---
description: >-
  Example code for the listreceivedbylabel  {disallowed} json-rpc method.
  Сomplete guide on how to use listreceivedbylabel  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# listreceivedbylabel {disallowed} - Dash

#### Parameters

`Confirmations` - number (int)

Optional.

The minimum number of confirmations a transaction must have before it is counted towards the balance. Use 0 to count unconfirmed transactions. Default is 1.

`addlocked` - bool

Optional.

Add the balance from InstantSend locked transactions.

`Include Empty` - bool

Optional.

Set to true to display accounts which have never received a payment.

Set to false (the default) to only include accounts which have received a payment.

Any account which has received a payment will be displayed even if its current balance is 0.

`Include Watch-Only` - bool

Optional.

If set to true, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.

If set to false (the default), treat watch-only addresses as if they didn't belong to this wallet.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "listreceivedbylabel",
"params": [null, null, null, null],
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
