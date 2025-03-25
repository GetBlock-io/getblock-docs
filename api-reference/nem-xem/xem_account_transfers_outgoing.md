---
description: >-
  Example code for the /account/transfers/outgoing rest method. Сomplete guide
  on how to use /account/transfers/outgoing rest in GetBlock.io Web3
  documentation.
---

# /account/transfers/outgoing - NEM

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
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/account/transfers/outgoing?address=NCXIQA4FF5JB6AMQ53NQ3ZMRD3X3PJEWDJJJIGHT&id=10919973' \
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "data": [
        {
            "meta": {
                "hash": {
                    "data": "871f84f02e97c0ccaea94f1aca54cfa793b65680212630dc866a914e9f1b29d5"
                },
                "height": 3105116,
                "id": 10165386,
                "innerHash": {}
            },
            "transaction": {
                "amount": 0,
                "deadline": 187915798,
                "fee": 321000000,
                "message": {
                    "payload": "7b2274797065223a362c202264657374696e6174696f6e223a2235353531363331373839373130313764666236333061373664323139333031643934376538623664656565333632383034393537333866346663353736666134222c20227061796c6f6164223a2261313030303030303030303030303030656439653666323631636439333833336230643031623031663063643632326438383330316461623066626261396363646164336166356365643633343033646235653766303662303835613935316664333837316463393932323165643337626338383131636665626533333230616230326333653861386534383634303235353531363331373839373130313764666236333061373664323139333031643934376538623664656565333632383034393537333866346663353736666134303030303030303030316138343334323030303030303030303030303030303030313030303030303030303030303030366161333961323561323931366337333462356433636631366165643732646664343462336536373639313433366165633233363231333766616630643237613031227d",
                    "type": 1
                },
                "recipient": "NAQ7RCYM4PRUAKA7AMBLN4NPBJEJMRCHHJYAVA72",
                "signature": "dbe39c31ab1e94ecbdb5628293ae58593fa8c7e745ca0e88cd55f5e92380222ec9dd35ee9bebb06c62660bcdc9f20ef468666362d133d68270648fe237cfe503",
                "signer": "107051c28a2c009a83ae0861cdbff7c1cbab387c964cc433f7d191d9c3115ed7",
                "timeStamp": 187912198,
                "type": 257,
                "version": 1744830465
            }
        }
    ]
}
```
