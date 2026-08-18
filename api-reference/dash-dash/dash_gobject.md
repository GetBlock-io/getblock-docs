---
description: >-
  Example code for the gobject  {disallowed} json-rpc method. Сomplete guide on
  how to use gobject  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# gobject {disallowed} - Dash

#### Parameters

`method name` - string

None

`governance-hash` - string (hex)

Hash of the governance object.

`signal` - string

Vote signal: funding, valid, or delete

`outcome` - string

Vote outcome: yes, no, or abstain

`alias` - string

Alias of voting masternode

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "gobject",
"params": ["vote-many", null, null, null, null],
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
