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
        "txid": "1d51c696eab35d7dad7631516cc0845aaab317fe67cb182782752547541d30ea"
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": {
        "txid": "1d51c696eab35d7dad7631516cc0845aaab317fe67cb182782752547541d30ea",
        "version": 1,
        "vin": [
            {
                "txid": "89ccda696344f877ec04fe165db294c218e95ac44d14f147eeae6eb76d80c2b3",
                "vout": 1,
                "sequence": 4294967295,
                "n": 0,
                "addresses": ["bc1q4pa6r4p8deecx0fspfr8r9y04ja6j5arq4fvc4"],
                "isAddress": true,
                "value": "1186802"
            }
        ],
        "vout": [
            {
                "value": "32323",
                "n": 0,
                "hex": "0014e8df018c7e326cc253faac7e46cdc51e68542c42",
                "addresses": ["bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq"],
                "isAddress": true
            }
        ],
        "blockHash": "00000000000000000002260ebdc1d4355c5d5e3db4dec86190494f50485cf941",
        "blockHeight": 967397,
        "confirmations": 91,
        "blockTime": 1789643130,
        "size": 222,
        "vsize": 141,
        "value": "1186633",
        "valueIn": "1186802",
        "fees": "169"
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
