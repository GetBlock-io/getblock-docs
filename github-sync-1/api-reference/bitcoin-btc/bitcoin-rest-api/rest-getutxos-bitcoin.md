---
description: >-
  Example code for the rest/getutxos REST endpoint. Complete guide on how to use the
  rest/getutxos REST endpoint in the GetBlock Web3 documentation.
---

# rest/getutxos - Bitcoin

This endpoint asks whether specific outpoints are unspent, and returns the ones that are. Outpoints are given as `txid-n` pairs and may be chained in a single path. Inserting `checkmempool` considers unconfirmed spends as well.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| checkmempool | string | path | No | Literal segment. When present, outputs already spent in the mempool are treated as spent |
| outpoints | string | path | Yes | One or more `txid-n` pairs separated by `/` |
| format | string | path | Yes | Response format appended to the last outpoint: `json`, `hex`, or `bin` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/getutxos/checkmempool/98bde4a7f407769028cbe725695d388324ab6b83fab4f310005082dd9b8b2de9-0.json'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/getutxos/checkmempool/98bde4a7f407769028cbe725695d388324ab6b83fab4f310005082dd9b8b2de9-0.json'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/getutxos/checkmempool/98bde4a7f407769028cbe725695d388324ab6b83fab4f310005082dd9b8b2de9-0.json')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "chainHeight": 967816,
    "chaintipHash": "000000000000000000019c481c042c0625abe1dc04fe26795e2e24b152791e5f",
    "bitmap": "1",
    "utxos": [
        {
            "height": 967816,
            "value": 0.0011348,
            "scriptPubKey": {
                "asm": "1 4507d40a378867a30aa9c77c02bf59852771d2e65ebef9e176c9951ef451d92b",
                "desc": "rawtr(4507d40a378867a30aa9c77c02bf59852771d2e65ebef9e176c9951ef451d92b)#ecmzx8gj",
                "hex": "51204507d40a378867a30aa9c77c02bf59852771d2e65ebef9e176c9951ef451d92b",
                "address": "bc1pg5ragz3h3pn6xz4fca7q906es5nhr5hxt6l0nctkex23aaz3my4sl5g9ax",
                "type": "witness_v1_taproot"
            }
        }
    ]
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| chainHeight | numeric | Height at which the query was answered |
| chaintipHash | string | Hash of the chain tip at that height |
| bitmap | string | One character per requested outpoint: `1` if unspent, `0` if spent. Order matches the request |
| utxos | array | One entry per unspent outpoint, in request order |
| utxos[].height | numeric | Height of the block that created the output |
| utxos[].value | numeric | Output value in BTC |
| utxos[].scriptPubKey | object | The locking script, with `asm`, `desc`, `hex`, `type`, and `address` |

{% hint style="warning" %}
**`bitmap` is the part to read.** Spent outpoints are omitted from `utxos` entirely, so the array does not line up with the request by index. Walk `bitmap` character by character: a `1` means the next entry in `utxos` belongs to that outpoint, a `0` means it was spent and has no entry.

This endpoint answers only about outpoints you already know. It cannot list the outputs belonging to an address; for that use Blockbook [api/v2/utxo](../bitcoin-blockbook-rest-api/api-v2-utxo-bitcoin.md).

Without the `checkmempool` segment, an output spent by an unconfirmed transaction is still reported as unspent.
{% endhint %}

## Use Cases

* **Spend Checks**: Confirm an output is still unspent before building a transaction
* **Double-Spend Detection**: Use `checkmempool` to catch outputs already spent by a pending transaction
* **Batch Validation**: Check several outpoints in one request

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The path is malformed, or a hash or height is invalid |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No object matches the requested hash, height, or outpoint |
| 500 | Internal error | The node failed to process the request |
