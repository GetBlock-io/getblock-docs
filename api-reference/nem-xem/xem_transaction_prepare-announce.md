---
description: >-
  Example code for the /transaction/prepare-announce rest method. Сomplete guide
  on how to use /transaction/prepare-announce rest in GetBlock.io Web3
  documentation.
---

# /transaction/prepare-announce - NEM

#### Parameters

`requestPrepareAnnounce` -

A RequestPrepareAnnounce JSON object. A RequestPrepareAnnounce object is used to transfer transaction data and a private key to NIS in order to initiate and broadcast a transaction.

#### Request

```java
curl --location --request POST 'https://xem.getblock.io/transaction/prepare-announce'
 --header 'x-api-key: YOUR-API-KEY'
 --header 'Content-Type: application/json'
 --data-raw '{requestPrepareAnnounce':{'transaction': None, 'timeStamp': 9111526, 'amount': 1000000000, 'fee': 50000, 'recipient': 'TDGIMREMR5NSRFUOMPI5OOHLDATCABNPC5ID2SVA', 'type': 257, 'deadline': 9154726, 'message': {'payload': '74657374207472616e73616374696f6e', 'type': 1}, 'version': -1744830463, 'signer': 'a1aaca6c17a24252e674d155713cdf55996ad00175be4af02a20c67b59f9fe8a', 'privateKey': '68e4f79f886927de698df4f857de2aada41ccca6617e56bb0d61623b35b08cc0'}'
```

#### Response

```java
{
    "type": 1,
    "code": 1,
    "message": "SUCCESS",
    "transactionHash": {
        "data": "c1786437336da077cd572a27710c40c378610e8d33880bcb7bdb0a42e3d35586"
    },
    "innerTransactionHash": {
        "data": "cc317a7674d56352b4c711096a7594bd11908bf518293a191fc2faa12eac0fbb"
    }
}
```
