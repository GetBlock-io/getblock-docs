---
description: >-
  Example code for the /tryLocateTx json-rpc method. Сomplete guide on how to
  use /tryLocateTx json-rpc in GetBlock.io Web3 documentation.
---

# /tryLocateTx - The Open Network (TON)

#### Parameters

`source` - query

string, required

`destination` - query

string, required

`created_lt` - query

integer, required

#### Request

```java
curl --location --request GET 'https://ton.getblock.io/mainnet/tryLocateTx?source=EQC1CQH3A1RA5Vmk30ayYByoVfF-m44MKiQiWhOJWXLtPCn3&destination=EQDXZ2c5LnA12Eum-DlguTmfYkMOvNeFCh4rBD0tgmwjcFI-&created_lt=38633831000014' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "ok": true,
    "result": {
        "@type": "raw.transaction",
        "data": "te6cckECBwEAAZsAA7V9dnZzkucDXYS6b4OWC5OZ9iQw6814UKHisEPS2CbCNwAAAjIyPF589fePvVTf0P9YAUBJ0jI2r9SuhIb7iHxJSSnVvvf7cb+AAAIyMjxefOZJGRtQABRh4+MIAQIDAQGgBACCchzmwteJVyBJcENRlZZH8Oi2ft5J/B/kpYSthcnF0+8C4/kLsByLHWbMgJ2sHob/PrxDgoZvCL19En9BntzL1EcCEwwI0nCe2GHj4xEFBgDHSAFqEgPuBqiByrNJvo1kwDlQq+L9NxwYVEhEtCcSsuXaeQA12dnOS5wNdhLpvg5YLk5n2JDDrzXhQoeKwQ9LYJsI3A0nCewGFFhgAABGRkeLz5zJIyNqapk7bZDNeODzgsz4QACcQHvoS4QAAAAAAAAAAB0AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFvAAAAAAAAAAAAAAAABLUUtpEnlC4z33SeGHxRhIq/htUa7i3D8ghbwxhQTn44E4jD2Og==",
        "fee": "991000",
        "in_msg": {
            "@type": "raw.message",
            "body_hash": "7o7+mXazXzO/gmvAfXXomeBLFDM6g+q7CICuIxTCW5I=",
            "created_lt": "38633831000014",
            "destination": "EQDXZ2c5LnA12Eum-DlguTmfYkMOvNeFCh4rBD0tgmwjcFI-",
            "fwd_fee": "666672",
            "ihr_fee": "0",
            "message": "1TJ22yGa8cHnBZnw",
            "msg_data": {
                "@type": "msg.dataRaw",
                "body": "te6cckEBAQEADgAAGNUydtshmvHB5wWZ8C7L/gM=",
                "init_state": ""
            },
            "source": "EQC1CQH3A1RA5Vmk30ayYByoVfF-m44MKiQiWhOJWXLtPCn3",
            "value": "4833915"
        },
        "other_fee": "991000",
        "out_msgs": [],
        "storage_fee": "0",
        "transaction_id": {
            "@type": "internal.transactionId",
            "hash": "H83yZHoxBmi+aBZxBN0J/jesYlJPZm7rci5tlA45Yqg=",
            "lt": "38633831000015"
        },
        "utime": 1687261621
    }
}
```
