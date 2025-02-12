---
description: >-
  Example code for the /detectAddress json-rpc method. Сomplete guide on how to
  use /detectAddress json-rpc in GetBlock.io Web3 documentation.
---

# /detectAddress - The Open Network (TON)

#### Parameters

`address` - query

required, string

Identifier of target TON account in any form.

#### Request

```java
curl --location --request GET 'https://ton.getblock.io/mainnet//detectAddress?address=EQDXZ2c5LnA12Eum-DlguTmfYkMOvNeFCh4rBD0tgmwjcFI-' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "ok": true,
    "result": {
        "bounceable": {
            "b64": "EQDXZ2c5LnA12Eum+DlguTmfYkMOvNeFCh4rBD0tgmwjcFI+",
            "b64url": "EQDXZ2c5LnA12Eum-DlguTmfYkMOvNeFCh4rBD0tgmwjcFI-"
        },
        "given_type": "friendly_bounceable",
        "non_bounceable": {
            "b64": "UQDXZ2c5LnA12Eum+DlguTmfYkMOvNeFCh4rBD0tgmwjcA/7",
            "b64url": "UQDXZ2c5LnA12Eum-DlguTmfYkMOvNeFCh4rBD0tgmwjcA_7"
        },
        "raw_form": "0:d76767392e7035d84ba6f83960b9399f62430ebcd7850a1e2b043d2d826c2370",
        "test_only": false
    }
}
```
