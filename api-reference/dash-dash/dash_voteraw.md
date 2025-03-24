---
description: >-
  Example code for the voteraw  {disallowed} json-rpc method. Сomplete guide on
  how to use voteraw  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# voteraw {disallowed} - Dash

#### Parameters

`masternode-collateral-tx-hash` - string (hex)

Hash of the masternode collateral transaction.

`masternode-collateral-tx-index` - string

Index of the masternode collateral transaction.

`governance-hash` - string (hex)

Hash of the governance object.

`signal` - string

Vote signal: funding, valid, or delete

`outcome` - string

Vote outcome: yes, no, or abstain

`time` - int64\_t

Create time

`vote-sig` - string (base64)

The vote signature created by external application (i.e. Dash Masternode Tool or dashmnb).

Must match the Dash Core (governance vote signature format).

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "voteraw",
"params": [null, null, null, null, null, null, null],
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
