---
description: >-
  Example code for the api/v2/tx-specific REST method. Complete guide on how to use the
  api/v2/tx-specific REST method in the GetBlock Web3 documentation.
---

# api/v2/tx-specific - Dogecoin

This endpoint returns the transaction exactly as the Dogecoin Core node reports it, in the node's own JSON shape rather than the indexer's normalized schema.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| txid | string | path | Yes | The transaction id |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "txid": "d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed",
    "hash": "d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed",
    "size": 225,
    "vsize": 225,
    "version": 1,
    "locktime": 0,
    "vin": [
        {
            "txid": "9af546f78a4cae9031954de80203cff6100c9c22744a4827738713595a1c588a",
            "vout": 0,
            "scriptSig": {
                "asm": "304402205e1ce8928d45e568b973b1617efbc19c5f30d16414adebe3ab45798e6ad8aefa...",
                "hex": "47304402205e1ce8928d45e568b973b1617efbc19c5f30d16414adebe3ab45798e6ad8aefa..."
            },
            "sequence": 4294967295
        }
    ],
    "vout": [
        {
            "value": 1806478.18052655,
            "n": 0,
            "scriptPubKey": {
                "asm": "OP_DUP OP_HASH160 986ae75df449ca5e04503c0baa69bbeca93fcf61 OP_EQUALVERIFY OP_CHECKSIG",
                "hex": "76a914986ae75df449ca5e04503c0baa69bbeca93fcf6188ac",
                "reqSigs": 1,
                "type": "pubkeyhash",
                "addresses": [
                    "DK31KDnpWXT5DyhZgrYY86VdQ92pp7hXmP"
                ]
            }
        }
    ],
    "blockhash": "afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf",
    "confirmations": 32,
    "time": 1789703885,
    "blocktime": 1789703885
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| txid | string | The transaction id |
| hash | string | Transaction hash, equal to txid on Dogecoin |
| size | integer | Serialized size in bytes |
| vsize | integer | Virtual size, equal to size on Dogecoin |
| locktime | integer | Transaction lock time |
| vin[].scriptSig | object | Full unlocking script, with asm and hex |
| vout[].value | number | Output value in DOGE as a decimal, not in koinu |
| vout[].scriptPubKey | object | Full node-native locking script, with asm, hex, type, and addresses |
| blockhash | string | Hash of the containing block |
| confirmations | integer | Number of confirmations |
| blocktime | integer | Block time as a Unix timestamp |

{% hint style="warning" %}
Values under `vout` are denominated in **DOGE as decimal numbers**, while the normalized [api/v2/tx](api-v2-tx-dogecoin.md) endpoint reports **koinu as integer strings**. The same output above appears as `1806478.18052655` here and `180647818052655` there.

Dogecoin amounts are large, so parsing these decimals as a 64-bit float loses precision. Read the normalized endpoint's integer strings when exact amounts matter, and treat this endpoint's decimals as display values.
{% endhint %}

Although Dogecoin has no SegWit, the node still reports `vsize` and it is always equal to `size`.

## Use Cases

* **Node Parity**: Read the node's exact transaction JSON for compatibility
* **Script Inspection**: Access `asm` and `reqSigs` fields absent from the normalized schema
* **Migration**: Match output from a direct node integration during a switch
* **Debugging**: Compare normalized and node-native views of a transaction

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
