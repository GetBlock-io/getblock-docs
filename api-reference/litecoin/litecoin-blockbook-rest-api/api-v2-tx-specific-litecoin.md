---
description: >-
  Example code for the api/v2/tx-specific REST method. Complete guide on how to use the
  api/v2/tx-specific REST method in the GetBlock Web3 documentation.
---

# api/v2/tx-specific - Litecoin

This endpoint returns the transaction exactly as the Litecoin Core node reports it, in the node's own JSON shape rather than the indexer's normalized schema. Use it for fields that the shared schema does not carry, such as MWEB flags and full script decoding.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| txid | string | path | Yes | The transaction id |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/51a31c4236a99bfd17cda606f079c9c4814851119e7593ab7c739bd5972b1969'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/51a31c4236a99bfd17cda606f079c9c4814851119e7593ab7c739bd5972b1969'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/51a31c4236a99bfd17cda606f079c9c4814851119e7593ab7c739bd5972b1969')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "txid": "51a31c4236a99bfd17cda606f079c9c4814851119e7593ab7c739bd5972b1969",
    "hash": "51a31c4236a99bfd17cda606f079c9c4814851119e7593ab7c739bd5972b1969",
    "version": 2,
    "size": 73090,
    "vsize": 73090,
    "weight": 292360,
    "locktime": 0,
    "vin": [
        {
            "ismweb": false,
            "txid": "4c3d9548bb03a42551c10f2472e99cd86776ed00fd3bfb530c4466b93e7ddac7",
            "vout": 2121,
            "sequence": 4294967295
        }
    ],
    "vout": [
        {
            "ismweb": false,
            "value": 83.24080358,
            "n": 0,
            "scriptPubKey": {
                "asm": "OP_DUP OP_HASH160 182c25bf95cacc04fd35d1ea8ea49539251bd552 OP_EQUALVERIFY OP_CHECKSIG",
                "hex": "76a914182c25bf95cacc04fd35d1ea8ea49539251bd55288ac",
                "reqSigs": 1,
                "type": "pubkeyhash",
                "addresses": [
                    "LMRmNEdhi5F43vtW5qPdv1q9s7xsMp4XK5"
                ]
            }
        }
    ],
    "blockhash": "0fc24234c2379a5262023a4b4a653ff3a11df6bb1c12851aff820d27ea02b0b6",
    "confirmations": 17,
    "time": 1789699786,
    "blocktime": 1789699786
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| txid | string | The transaction id |
| hash | string | Witness transaction id, equal to txid for non-SegWit transactions |
| version | integer | Transaction version |
| size | integer | Serialized size in bytes |
| vsize | integer | Virtual size in vbytes |
| weight | integer | SegWit transaction weight |
| locktime | integer | Transaction lock time |
| vin[].ismweb | boolean | True when the input comes from the MWEB extension block |
| vout[].ismweb | boolean | True when the output is held in the MWEB extension block |
| vout[].value | number | Output value in LTC as a decimal, not in litoshis |
| vout[].scriptPubKey | object | Full node-native script, with asm, hex, type, and addresses |
| blockhash | string | Hash of the containing block |
| confirmations | integer | Number of confirmations |
| blocktime | integer | Block time as a Unix timestamp |

{% hint style="info" %}
`ismweb` is Litecoin-specific and has no equivalent in the normalized [api/v2/tx](api-v2-tx-litecoin.md) schema, which is shared across all coins. MWEB (MimbleWimble Extension Blocks, LIP-0002) holds confidential outputs whose amounts are not visible on the canonical chain, so an output with `ismweb: true` will not carry an ordinary address and value the way a regular output does. Integrations that total balances from raw outputs should check this flag.

Values under `vout` are denominated in LTC as decimal numbers, not in litoshis as integer strings; the normalized endpoint reports litoshis instead.
{% endhint %}

## Use Cases

* **Node Parity**: Read the node's exact transaction JSON for compatibility
* **Script Inspection**: Access `asm` and `reqSigs` fields absent from the normalized schema
* **MWEB Handling**: Detect inputs and outputs that live in the MimbleWimble extension block
* **Debugging**: Compare normalized and node-native views of a transaction

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The transaction id is not a valid 64-character hex string |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No transaction with the requested id is indexed |
| 500 | Internal error | The indexer failed to read the transaction |
