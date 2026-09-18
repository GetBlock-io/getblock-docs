---
description: >-
  Example code for the getTransaction WebSocket method. Complete guide on how to
  use the getTransaction WebSocket method in the GetBlock Web3 documentation.
---

# getTransaction - Litecoin

Returns a normalized transaction by its id, with inputs, outputs, addresses, values, and confirmation data. This is the WebSocket form of the REST [api/v2/tx](../litecoin-blockbook-rest-api/api-v2-tx-litecoin.md) endpoint.

## Parameters

| Parameter | Type   | Required | Description    |
| --------- | ------ | -------- | -------------- |
| txid      | string | Yes      | Transaction id |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>

# then send:
{
    "id": "getblock.io",
    "method": "getTransaction",
    "params": {
        "txid": "51a31c4236a99bfd17cda606f079c9c4814851119e7593ab7c739bd5972b1969"
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": {
        "txid": "51a31c4236a99bfd17cda606f079c9c4814851119e7593ab7c739bd5972b1969",
        "version": 2,
        "vin": [
            {
                "txid": "4c3d9548bb03a42551c10f2472e99cd86776ed00fd3bfb530c4466b93e7ddac7",
                "vout": 2121,
                "sequence": 4294967295,
                "n": 0,
                "addresses": ["LUnYgfpJMLM2h27wDcJypQHQhRXXrZrfvE"],
                "isAddress": true,
                "value": "720950"
            }
        ],
        "vout": [
            {
                "value": "8324080358",
                "n": 0,
                "hex": "76a914182c25bf95cacc04fd35d1ea8ea49539251bd55288ac",
                "addresses": ["LMRmNEdhi5F43vtW5qPdv1q9s7xsMp4XK5"],
                "isAddress": true
            }
        ],
        "blockHash": "0fc24234c2379a5262023a4b4a653ff3a11df6bb1c12851aff820d27ea02b0b6",
        "blockHeight": 3179800,
        "confirmations": 17,
        "blockTime": 1789699786,
        "size": 73090,
        "vsize": 73090,
        "value": "103517609950",
        "valueIn": "103517609950",
        "fees": "0"
    }
}
```

## Response Fields

| Field         | Type    | Description                                             |
| ------------- | ------- | ------------------------------------------------------- |
| txid          | string  | The transaction id                                      |
| vin           | array   | Inputs with addresses and values                        |
| vout          | array   | Outputs with values, scripts, and destination addresses |
| blockHeight   | integer | Block height, or -1 while unconfirmed                   |
| confirmations | integer | Number of confirmations. 0 while unconfirmed            |
| size          | integer | Serialized size in bytes                                |
| vsize         | integer | Virtual size in vbytes                                  |
| value         | string  | Total output value in litoshis                          |
| fees          | string  | Fee paid in litoshis                                    |

## Use Cases

* **Payment Verification**: Confirm the amount and destination of a deposit
* **Confirmation Checks**: Read the current depth of a transaction
* **Fee Analysis**: Compare fees paid against virtual size
* **Input Tracing**: Follow the addresses a transaction spends

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| error                     | Not found     | No transaction exists with the requested id       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
