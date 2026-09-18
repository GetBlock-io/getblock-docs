---
description: >-
  Example code for the getTransaction WebSocket method. Complete guide on how to
  use the getTransaction WebSocket method in the GetBlock Web3 documentation.
---

# getTransaction - Bitcoin

Returns a normalized transaction by its id, with inputs, outputs, addresses, values, and confirmation data in the indexer's unified schema. This is the WebSocket form of the REST [api/v2/tx](../bitcoin-blockbook-rest-api/api-v2-tx-bitcoin.md) endpoint.

## Parameters

| Parameter | Type   | Required | Description         |
| --------- | ------ | -------- | ------------------- |
| txid      | string | Yes      | Transaction id/hash |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/websocket

# then send:
{
    "id": "getblock.io",
    "method": "getTransaction",
    "params": {
        "txid": "8c1e3dec662d1f2a5e322ccef5eca263f98eb16723c6f990be0c88c1db113fb1"
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": {
        "txid": "8c1e3dec662d1f2a5e322ccef5eca263f98eb16723c6f990be0c88c1db113fb1",
        "version": 2,
        "lockTime": 860729,
        "vin": [
            {
                "txid": "0eb7b574373de2c88d0dc1444f49947c681d0437d21361f9ebb4dd09c62f2a66",
                "vout": 1,
                "sequence": 4294967293,
                "n": 0,
                "addresses": ["bc1qmgwnfjlda4ns3g6g3yz74w6scnn9yu2ts82yyc"],
                "isAddress": true,
                "value": "10106300"
            }
        ],
        "vout": [
            {
                "value": "175000",
                "n": 0,
                "hex": "76a914ecc999d554eaa3efa5e871c28f58b549c36ec51788ac",
                "addresses": ["1Nb1ykSD7J5k4RFjJQGsrD9gxBE6jzfNa9"],
                "isAddress": true
            }
        ],
        "blockHash": "00000000000000000000effeb0c4460480e6a347deab95332c63007a68646ee5",
        "blockHeight": 860730,
        "confirmations": 1,
        "blockTime": 1725956288,
        "size": 225,
        "vsize": 144,
        "value": "10063100",
        "valueIn": "10106300",
        "fees": "43200"
    }
}
```

## Response Fields

| Field         | Type    | Description                                                   |
| ------------- | ------- | --------------------------------------------------------------- |
| txid          | string  | Transaction id                                                |
| vin           | array   | Inputs, with the address and value of each spent output       |
| vout          | array   | Outputs, with value, script, and destination addresses        |
| blockHash     | string  | Hash of the containing block. Absent while unconfirmed        |
| blockHeight   | integer | Height of the containing block. `-1` while unconfirmed        |
| confirmations | integer | Number of confirmations. 0 while unconfirmed                  |
| blockTime     | integer | Block timestamp, or first-seen time for a mempool transaction |
| value         | string  | Total output value in satoshis                                |
| valueIn       | string  | Total input value in satoshis                                 |
| fees          | string  | Fee paid in satoshis                                          |
| rbf           | boolean | True when the transaction signals replace-by-fee              |

## Use Cases

* **Payment Verification**: Confirm the amount and destination of a deposit
* **Confirmation Checks**: Read the current depth of a transaction
* **Fee Analysis**: Compare fees paid against the transaction's virtual size
* **Input Tracing**: Follow the addresses and values a transaction spends

{% hint style="info" %}
For a mempool transaction, `blockTime` is when this indexer first saw the transaction, not a consensus timestamp, so it can differ between instances.
{% endhint %}

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| error                     | Not found     | No transaction exists with the requested id       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
