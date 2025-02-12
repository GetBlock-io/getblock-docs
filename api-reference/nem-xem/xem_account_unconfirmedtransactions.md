---
description: >-
  Example code for the /account/unconfirmedTransactions rest method. Сomplete
  guide on how to use /account/unconfirmedTransactions rest in GetBlock.io Web3
  documentation.
---

# /account/unconfirmedTransactions - NEM

#### Parameters

`address` -

The address of the account.

#### Request

```java
curl --location --request GET 'https://xem.getblock.io/account/unconfirmedTransactions?address=NCXIQA4FF5JB6AMQ53NQ3ZMRD3X3PJEWDJJJIGHT'
 --header 'x-api-key: YOUR-API-KEY' 
 --header 'Content-Type: application/json'
```

#### Response

```java
{
    "data": [
        {
            "meta": {
                "data": "d7c9e33421e43bf4a5d6e21304c8096c599142755d581bd6e9037f41545a5873",
                "transaction": null,
                "timeStamp": 9131839,
                "amount": 1000000000,
                "signature": "0acface77696a54340a7da8592750ea0410f62717d07e4df30e09718092521262465df5c4d98d32cd9d6e8699d66e016ec8db716d20090ad99cc16f7a6d13904",
                "fee": 2000000,
                "recipient": "TDGIMREMR5NSRFUOMPI5OOHLDATCABNPC5ID2SVA",
                "type": 257,
                "deadline": 9175039,
                "message": {
                    "payload": "",
                    "type": 1
                },
                "version": -1744830463,
                "signer": "a1aaca6c17a24252e674d155713cdf55996ad00175be4af02a20c67b59f9fe8a"
            }
        }
    ]
}
```
