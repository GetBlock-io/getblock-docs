---
description: >-
  Example code for the api/v2/tx-specific REST method. Complete guide on how to use the
  api/v2/tx-specific REST method in the GetBlock Web3 documentation.
---

# api/v2/tx-specific - Zcash

This endpoint returns the transaction exactly as the Zebra node reports it, in the node's own JSON shape. For Zcash it is the only Blockbook endpoint that shows a transaction's shielded components: Sapling spends and outputs, Orchard actions, and the value balance of each pool.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| txid | string | path | Yes | The transaction id |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/076a0119b89f661efa34e815079726b36b4cfbd0375f66af72f505c88bafde37'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/076a0119b89f661efa34e815079726b36b4cfbd0375f66af72f505c88bafde37'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/076a0119b89f661efa34e815079726b36b4cfbd0375f66af72f505c88bafde37')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "txid": "076a0119b89f661efa34e815079726b36b4cfbd0375f66af72f505c88bafde37",
    "version": 5,
    "versiongroupid": "26a7270a",
    "overwintered": true,
    "locktime": 0,
    "expiryheight": 3487829,
    "vin": [],
    "vout": [
        {
            "value": 15.03454842,
            "valueZat": 1503454842,
            "n": 0,
            "scriptPubKey": {
                "asm": "OP_DUP OP_HASH160 94748c0ca42e412783804af516617ff4cbb49de3 OP_EQUALVERIFY OP_CHECKSIG",
                "hex": "76a91494748c0ca42e412783804af516617ff4cbb49de388ac",
                "reqSigs": 1,
                "type": "pubkeyhash",
                "addresses": [
                    "t1XQZdZMnzXBcL8yx2PR27dSNrqctgwLgux"
                ]
            }
        }
    ],
    "vShieldedSpend": [
        {
            "cv": "...",
            "anchor": "...",
            "nullifier": "...",
            "rk": "...",
            "proof": "...",
            "spendAuthSig": "..."
        }
    ],
    "vShieldedOutput": [
        {
            "cv": "...",
            "cmu": "...",
            "ephemeralKey": "...",
            "encCiphertext": "...",
            "outCiphertext": "...",
            "proof": "..."
        }
    ],
    "valueBalance": 15.03469842,
    "valueBalanceZat": 1503469842,
    "orchard": {
        "actions": [],
        "valueBalance": 0.0,
        "valueBalanceZat": 0
    },
    "vjoinsplit": [],
    "size": 2763,
    "height": 3487790,
    "confirmations": 46,
    "blockhash": "00000000000f3c0bdf81aba0bf081c5c4882975cbc0a8ce35e70ddb5f265f1b1",
    "time": 1789741929,
    "blocktime": 1789741929,
    "in_active_chain": true
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| version | integer | Transaction version. 5 from NU5, 6 from NU6 |
| versiongroupid | string | Version group identifier for the transaction format |
| expiryheight | integer | Height after which the transaction can no longer be mined |
| vin | array | Transparent inputs, in node-native form |
| vout | array | Transparent outputs. `value` is in ZEC as a decimal; `valueZat` is the same amount as an exact integer in zatoshis |
| vShieldedSpend | array | Sapling spends. Each carries a nullifier and proof, never an amount or address |
| vShieldedOutput | array | Sapling outputs. Each carries encrypted note data, never an amount or address |
| valueBalance | number | Net value leaving the Sapling pool, in ZEC. Positive means value exits into the transparent side |
| valueBalanceZat | integer | The same Sapling value balance, in zatoshis |
| orchard | object | Orchard actions and the Orchard pool's own value balance |
| vjoinsplit | array | Legacy Sprout JoinSplits. Empty on modern transactions |

{% hint style="info" %}
`vShieldedSpend` and `vShieldedOutput` are truncated to one entry each, with their long hex fields shown as `"..."`; this transaction has two of each.

The fee is `sum(transparent inputs) + valueBalanceZat + orchard.valueBalanceZat - sum(transparent outputs)`. Here that is `0 + 1503469842 + 0 - 1503454842 = 15000` zatoshis, which matches the ZIP-317 conventional fee of 5,000 zatoshis per logical action for this transaction's three actions. The normalized [api/v2/tx](api-v2-tx-zcash.md) endpoint reports the same fee as `0`.

Each `vout` entry carries the amount twice: `value` in ZEC as a decimal, and `valueZat` in zatoshis as an integer. Read `valueZat` for arithmetic, since the decimal loses precision as a floating-point number.
{% endhint %}

## Use Cases

* **Shielded Fee Calculation**: Read each pool's value balance to compute a correct fee
* **Pool Flow Analysis**: See whether value is entering or leaving a shielded pool
* **Expiry Checks**: Read `expiryheight` to know when a pending transaction lapses
* **Node Parity**: Read the node's exact transaction JSON for compatibility

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
