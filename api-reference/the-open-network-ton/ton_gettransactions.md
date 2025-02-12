---
description: >-
  Example code for the /getTransactions json-rpc method. Сomplete guide on how
  to use /getTransactions json-rpc in GetBlock.io Web3 documentation.
---

# /getTransactions - The Open Network (TON)

#### Parameters

`address` - query

string, required

Identifier of target TON account in any form.

`limit` - query

integer, optional

Maximum number of transactions in response.

`lt` - query

integer, optional

Logical time of transaction to start with, must be sent with hash.

`None` - query

string, optional

Hash of transaction to start with, in base64 or hex encoding , must be sent with lt.

`to_lt` - query

integer, optional

Logical time of transaction to finish with (to get tx from lt to to\_lt).

`archival` - query

boolean, optional

By default getTransaction request is processed by any available liteserver. If archival=true only liteservers with full history are used.

#### Request

```java
curl --location --request GET 'https://ton.getblock.io/mainnet/getTransactions?address=EQDXZ2c5LnA12Eum-DlguTmfYkMOvNeFCh4rBD0tgmwjcFI-&limit=2' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "ok": true,
    "result": [
        {
            "@type": "raw.transaction",
            "data": "te6cckECCwEAAqgAA7V9dnZzkucDXYS6b4OWC5OZ9iQw6814UKHisEPS2CbCNwAAAjKFbN7IHnBafzlrY2bbqfoKFqB1CYkMxu5lTuZQIqNrykfLvDwQAAIyhRrasHZJKmnwADRr9oeoAQIDAgHgBAUAgnKZlWP/C4YETVjQyuLD2dq4IWZQqjzXR11YwK/zgl6WIYA19K2iy1r8hLAmrnprvf/o4dDuVrGtVjG/8tH9xb9sAg8MUcYZk88EQAkKAeGIAa7OznJc4Guwl03wcsFycz7Ehh15rwoUPFYIelsE2EbgBdh8UojZH3eD1NCPuboAx4rnKnuELuYWxSviyfHdo0Keywp3TY/C47UQidxCwUX4PrlBxhUU7Sul5TaGUIPU0EFNTRi7JJU2sAAAbKAAHAYBAd8HAWhiAFBN54YkbWG1yKIszuQL1pUmdnao82RIraWakeDJEc8SoTEtAAAAAAAAAAAAAAAAAAABCAGxaAGuzs5yXOBrsJdN8HLBcnM+xIYdea8KFDxWCHpbBNhG4QAoJvPDEjaw2uRRFmdyBetKkzs7VHmyJFbSzUjwZIjniVCYloAABiValAAARlCtm9kEySVNPsAIALgPin6lAAAAAAAAAABwnylc1fAACAG9YW1s5KC/bxqt/LoPHKSlyjhjhtL4kkhIo7i/WRbdIwA12dnOS5wNdhLpvg5YLk5n2JDDrzXhQoeKwQ9LYJsI3AICAAAAAACdQZ2DE4gAAAAAAAAAABEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIABvyY4B8EwlWlgAAAAAAAIAAAAAAANse0+kBNKHksJOoZpSfKQKE95bV9zDEmZMbT47SrOZVECQLQw3gECb",
            "fee": "7496071",
            "in_msg": {
                "@type": "raw.message",
                "body_hash": "w/DuzE0gSARe6PVn5EXhIPHeeH7P20RZ8h3OOjC9mp4=",
                "created_lt": "0",
                "destination": "EQDXZ2c5LnA12Eum-DlguTmfYkMOvNeFCh4rBD0tgmwjcFI-",
                "fwd_fee": "0",
                "ihr_fee": "0",
                "message": "uw+KURsj7vB6mhH3N0AY8VzlT3CF3MLYpXxZPju0aFPZYU7psfhcdqIRO4hYKL8H1yg4wqKdpXS8ptDKEHqaCCmpoxdkkqbWAAANlAAD",
                "msg_data": {
                    "@type": "msg.dataRaw",
                    "body": "te6cckEBAwEA5gABnLsPilEbI+7wepoR9zdAGPFc5U9whdzC2KV8WT47tGhT2WFO6bH4XHaiETuIWCi/B9coOMKinaV0vKbQyhB6mggpqaMXZJKm1gAADZQAAwEBaGIAUE3nhiRtYbXIoizO5AvWlSZ2dqjzZEitpZqR4MkRzxKhMS0AAAAAAAAAAAAAAAAAAAECALgPin6lAAAAAAAAAABwnylc1fAACAG9YW1s5KC/bxqt/LoPHKSlyjhjhtL4kkhIo7i/WRbdIwA12dnOS5wNdhLpvg5YLk5n2JDDrzXhQoeKwQ9LYJsI3AICAAAAAFqIi0U=",
                    "init_state": ""
                },
                "source": "",
                "value": "0"
            },
            "other_fee": "7496000",
            "out_msgs": [
                {
                    "@type": "raw.message",
                    "body_hash": "hvYWYf6TJDOM6eJKTvnOltIzvi3Wn6SOFvZzNfgwTE0=",
                    "created_lt": "38656162000002",
                    "destination": "EQCgm88MSNrDa5FEWZ3IF60qTOztUebIkVtLNSPBkiOeJXB9",
                    "fwd_fee": "1224010",
                    "ihr_fee": "0",
                    "message": "D4p+pQAAAAAAAAAAcJ8pXNXwAAgBvWFtbOSgv28arfy6Dxykpco4Y4bS+JJISKO4v1kW3SMANdnZzkucDXYS6b4OWC5OZ9iQw6814UKHisEPS2CbCNwCAgAAAAA=",
                    "msg_data": {
                        "@type": "msg.dataRaw",
                        "body": "te6cckEBAQEAXgAAuA+KfqUAAAAAAAAAAHCfKVzV8AAIAb1hbWzkoL9vGq38ug8cpKXKOGOG0viSSEijuL9ZFt0jADXZ2c5LnA12Eum+DlguTmfYkMOvNeFCh4rBD0tgmwjcAgIAAAAA5zOMjA==",
                        "init_state": ""
                    },
                    "source": "EQDXZ2c5LnA12Eum-DlguTmfYkMOvNeFCh4rBD0tgmwjcFI-",
                    "value": "640000000"
                }
            ],
            "storage_fee": "71",
            "transaction_id": {
                "@type": "internal.transactionId",
                "hash": "g1DNZDyxC6lil8O0lHMWG00TmdPOpYA7FlxnHrppxI8=",
                "lt": "38656162000001"
            },
            "utime": 1687332511
        }
    ]
}
```
