---
description: >-
  Example code for the getTransaction WebSocket method. Complete guide on how to use
  the getTransaction WebSocket method in the GetBlock Web3 documentation.
---

# getTransaction - Dogecoin

Returns a normalized transaction by its id, with inputs, outputs, addresses, values, and confirmation data. This is the WebSocket form of the REST [api/v2/tx](../dogecoin-blockbook-rest-api/api-v2-tx-dogecoin.md) endpoint.

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
        "txid": "d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed"
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": {
        "txid": "d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed",
        "version": 1,
        "vin": [
            {
                "txid": "9af546f78a4cae9031954de80203cff6100c9c22744a4827738713595a1c588a",
                "sequence": 4294967295,
                "n": 0,
                "addresses": ["DL4iyB4jtVNtwPsti24ron4prY2UZoUjG3"],
                "isAddress": true,
                "value": "500000000000000"
            }
        ],
        "vout": [
            {
                "value": "180647818052655",
                "n": 0,
                "spent": true,
                "hex": "76a914986ae75df449ca5e04503c0baa69bbeca93fcf6188ac",
                "addresses": ["DK31KDnpWXT5DyhZgrYY86VdQ92pp7hXmP"],
                "isAddress": true
            }
        ],
        "blockHash": "afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf",
        "blockHeight": 6378930,
        "confirmations": 32,
        "blockTime": 1789703885,
        "size": 225,
        "value": "499999999659500",
        "valueIn": "500000000000000",
        "fees": "340500"
    }
}
```

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| txid | string | The transaction id |
| vin | array | Inputs with addresses and values |
| vout | array | Outputs with values, scripts, and destination addresses |
| vout[].spent | boolean | True when the output has already been spent |
| blockHeight | integer | Block height, or -1 while unconfirmed |
| confirmations | integer | Number of confirmations. 0 while unconfirmed |
| size | integer | Serialized size in bytes |
| value | string | Total output value in koinu |
| fees | string | Fee paid in koinu |

{% hint style="info" %}
Dogecoin has no SegWit, so there is no `vsize` field; `size` is the only size reported and fee rates are per byte.
{% endhint %}

## Use Cases

* **Payment Verification**: Confirm the amount and destination of a deposit
* **Confirmation Checks**: Read the current depth of a transaction
* **Fee Inspection**: Read the computed fee for a transaction
* **Spend Tracking**: Check whether outputs are already spent

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| error | Not found | No transaction exists with the requested id |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
