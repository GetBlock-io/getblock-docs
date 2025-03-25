---
description: >-
  Example code for the /account/transfers/all rest method. Сomplete guide on how
  to use /account/transfers/all rest in GetBlock.io Web3 documentation.
---

# /account/transfers/all - NEM

#### Parameters

`address` -

The address of the account.

`hash` -

The 256 bit sha3 hash of the transaction up to which transactions are returned.

The parameter is optional. When it's not present, the request will return newest transactions according to the above criteria. When hash is supplied as second parameter, the request will return up to 25 transactions that appeared directly before the transaction that has the supplied hash sorted according to the above criteria.

`id` -

The transaction id up to which transactions are returned. The parameter is optional. When an id is supplied as third parameter, the request will return up to 25 transactions that appeared directly before the transaction that has the supplied id sorted according to the above criteria.

#### Request

```java
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/account/transfers/all?address=NCXIQA4FF5JB6AMQ53NQ3ZMRD3X3PJEWDJJJIGHT&id=10919973' \
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "data": [
        {
            "meta": {
                "hash": {
                    "data": "87e2033d75b0e9d7445d9a617f70e45e2af116aed7c6895d586822316811e318"
                },
                "height": 3398888,
                "id": 10918761,
                "innerHash": {}
            },
            "transaction": {
                "amount": 1000000,
                "deadline": 205692008,
                "fee": 300000,
                "message": {
                    "payload": "444f4e5445204449474954414c31c2aeefb88f0a68747470733a2f2f6d6f62696c652e747769747465722e636f6d2f6469676974616c31720a68747470733a2f2f7777772e62657867726f75702e696e666f0a68747470733a2f2f6d6f62696c652e6265782e676c6f62616c0a68747470733a2f2f646578782e676c6f62616c0a68747470733a2f2f6962696e2e676c6f62616c0a",
                    "type": 1
                },
                "mosaics": [
                    {
                        "mosaicId": {
                            "name": "ch0459804891b",
                            "namespaceId": "asset"
                        },
                        "quantity": 2000000
                    }
                ],
                "recipient": "NCXIQA4FF5JB6AMQ53NQ3ZMRD3X3PJEWDJJJIGHT",
                "signature": "bda7036a0aff76fa20ff9e27e9e63375a018328703c6294ac869ffb3a597526c9509cf92057e0893e9c1b0de656ecc19832913f115b5679f69e14ead23fe5c0c",
                "signer": "0b917422ca9259c9acf379f9ff804886768dea2314b670b64732cb73a72576ca",
                "timeStamp": 205684808,
                "type": 257,
                "version": 1744830466
            }
        }
    ]
}
```
