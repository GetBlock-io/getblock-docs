---
description: >-
  Example code for the getgovernanceinfo json-rpc method. Сomplete guide on how
  to use getgovernanceinfo json-rpc in GetBlock.io Web3 documentation.
---

# getgovernanceinfo - Dash

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getgovernanceinfo",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "governanceminquorum": 10,
        "lastsuperblock": 1528672,
        "nextsuperblock": 1545288,
        "proposalfee": 5.0,
        "superblockcycle": 16616
    }
}
```
