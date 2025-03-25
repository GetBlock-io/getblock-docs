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
curl --location --request POST 'https://go.getblock.io/YOUR-ACCESS-TOKEN/transaction/announce' \
--header 'Content-Type: application/json' \
--data-raw '{
  "data": "010100000100000000000000200000002b76078fa709bbe6752222b215abc7ec0152ffe831fb4f9aed3e7749a425900a00093d0000000000000000002800000054444e46555946584f5353334e4e4c4f35465a5348535a49354c33374b4e5149454850554d584c54c0d45407000000000b00000001000000030000000c3215",
  "signature": "db2473513c7f0ce9f8de6345f0fbe773dc687eb571123d08eab4d98f96849eaeb63fa8756fb6c59d9b9d0e551537c1cdad4a564747ff9291db4a88b65c97c10d"
}'
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
