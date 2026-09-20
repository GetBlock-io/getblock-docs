---
description: >-
  Example code for the getTransaction WebSocket method. Complete guide on how to use
  the getTransaction WebSocket method in the GetBlock Web3 documentation.
---

# getTransaction - Zcash

Returns a normalized transaction by its id, with transparent inputs, transparent outputs, addresses, values, and confirmation data. This is the WebSocket form of the REST [api/v2/tx](../zcash-blockbook-rest-api/api-v2-tx-zcash.md) endpoint.

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| txid | string | Yes | Transaction id |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>

# then send:
{
    "id": "getblock.io",
    "method": "getTransaction",
    "params": {
        "txid": "076a0119b89f661efa34e815079726b36b4cfbd0375f66af72f505c88bafde37"
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": {
        "txid": "076a0119b89f661efa34e815079726b36b4cfbd0375f66af72f505c88bafde37",
        "version": 5,
        "vin": [],
        "vout": [
            {
                "value": "1503454842",
                "n": 0,
                "spent": true,
                "hex": "76a91494748c0ca42e412783804af516617ff4cbb49de388ac",
                "addresses": [
                    "t1XQZdZMnzXBcL8yx2PR27dSNrqctgwLgux"
                ],
                "isAddress": true
            }
        ],
        "blockHash": "00000000000f3c0bdf81aba0bf081c5c4882975cbc0a8ce35e70ddb5f265f1b1",
        "blockHeight": 3487790,
        "confirmations": 2048,
        "blockTime": 1789741929,
        "size": 2763,
        "value": "1503454842",
        "valueIn": "0",
        "fees": "0"
    }
}
```

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| txid | string | The transaction id |
| version | integer | Transaction version. 5 from NU5, 6 from NU6 |
| vin | array | Transparent inputs. Empty when every input is shielded |
| vout | array | Transparent outputs with values and destination addresses |
| blockHeight | integer | Block height, or -1 while unconfirmed |
| confirmations | integer | Number of confirmations. 0 while unconfirmed |
| size | integer | Serialized size in bytes |
| value | string | Total transparent output value in zatoshis |
| valueIn | string | Total transparent input value in zatoshis |
| fees | string | `valueIn` minus `value`. Correct only when the transaction has no shielded component |

{% hint style="danger" %}
**The response above is a shielded transaction, and its `fees` field reads `0`.** That is wrong, and it is the single trap to know about on Zcash.

`vin` is empty and `valueIn` is `"0"` because the value came out of the Sapling pool, which the indexer cannot see. The real fee is **15,000 zatoshis**: the pool released 1,503,469,842 zatoshis and the transparent output received 1,503,454,842.

The WebSocket interface has no method that exposes shielded components, so the value balances must be read from REST [api/v2/tx-specific](../zcash-blockbook-rest-api/api-v2-tx-specific-zcash.md) or JSON-RPC [getrawtransaction](../zcash-json-rpc-api/getrawtransaction-zcash.md). Never compute a Zcash fee from this method alone.
{% endhint %}

## Use Cases

* **Payment Verification**: Confirm the amount and destination of a transparent deposit
* **Confirmation Checks**: Read the current depth of a transaction
* **Spend Tracking**: Check whether outputs are already spent

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| error | Not found | No transaction exists with the requested id |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
