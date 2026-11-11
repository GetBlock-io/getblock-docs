---
description: >-
  Example code for the rest/tx REST endpoint. Complete guide on how to use the
  rest/tx REST endpoint in the GetBlock Web3 documentation.
---

# rest/tx - Bitcoin

This endpoint returns a transaction by its id, decoded into JSON. The same transaction is available as raw hex at `.hex` and as raw binary at `.bin`.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| txid | string | path | Yes | The transaction id |
| format | string | path | Yes | Response format appended to the txid: `json`, `hex`, or `bin` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/tx/98bde4a7f407769028cbe725695d388324ab6b83fab4f310005082dd9b8b2de9.json'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/tx/98bde4a7f407769028cbe725695d388324ab6b83fab4f310005082dd9b8b2de9.json'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/tx/98bde4a7f407769028cbe725695d388324ab6b83fab4f310005082dd9b8b2de9.json')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "txid": "98bde4a7f407769028cbe725695d388324ab6b83fab4f310005082dd9b8b2de9",
    "hash": "290ba098c730ca593384647c6c55f4233a60fbdaa9055fd1fe54f51f49a30ad0",
    "version": 2,
    "size": 205,
    "vsize": 154,
    "weight": 616,
    "locktime": 0,
    "vin": [
        {
            "txid": "5be5b34a8291c73bd91dd71a36306403151ce55deee37a3008232d3fc6c8a29a",
            "vout": 1,
            "scriptSig": {
                "asm": "",
                "hex": ""
            },
            "txinwitness": [
                "0643405bf90e906d26ef8b871201657f..."
            ],
            "sequence": 4294967295
        }
    ],
    "vout": [
        {
            "value": 0.0011348,
            "n": 0,
            "scriptPubKey": {
                "asm": "1 4507d40a378867a30aa9c77c02bf59852771d2e65ebef9e176c9951ef451d92b",
                "desc": "rawtr(4507d40a378867a30aa9c77c02bf59852771d2e65ebef9e176c9951ef451d92b)#ecmzx8gj",
                "hex": "51204507d40a378867a30aa9c77c02bf59852771d2e65ebef9e176c9951ef451d92b",
                "address": "bc1pg5ragz3h3pn6xz4fca7q906es5nhr5hxt6l0nctkex23aaz3my4sl5g9ax",
                "type": "witness_v1_taproot"
            }
        }
    ],
    "blockhash": "000000000000000000019c481c042c0625abe1dc04fe26795e2e24b152791e5f",
    "hex": "020000000001019aa2c8c63f2d2308307ae3ee5de51c15036430361ad71dd93b..."
}
```

The `vin`, `vout`, and witness arrays are truncated to one entry, and `hex` is abbreviated.

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| txid | string | The transaction id |
| hash | string | The witness transaction id. Differs from `txid` for SegWit transactions |
| version | numeric | Transaction version |
| size | numeric | Serialized size in bytes |
| vsize | numeric | Virtual size in vbytes, the basis for fee rates |
| weight | numeric | Transaction weight |
| locktime | numeric | Transaction lock time |
| vin | array | Inputs, each with the outpoint it spends and its witness |
| vout | array | Outputs, each with a value in BTC and a decoded `scriptPubKey` |
| blockhash | string | Hash of the containing block, when the transaction is confirmed |
| hex | string | The complete serialized transaction, hex-encoded |

{% hint style="warning" %}
The response carries **no `fee` field and no confirmation count**, because the node would have to look up each input's previous output to compute them. Fetch the inputs' transactions and subtract, or use the JSON-RPC [getrawtransaction](../bitcoin-btc-json-rpc-api/getrawtransaction-bitcoin.md) method with verbosity `2`, which returns a `fee` directly.

Output values are in **BTC as decimals**, not satoshis. Parsing them as floats loses precision; the Blockbook [api/v2/tx](../bitcoin-blockbook-rest-api/api-v2-tx-bitcoin.md) endpoint reports integer satoshi strings instead.
{% endhint %}

## Use Cases

* **Transaction Inspection**: Read a decoded transaction without running a node
* **Raw Retrieval**: Fetch the serialized transaction with the `.hex` or `.bin` form
* **Script Analysis**: Read each output's `asm`, `desc`, and `address`
* **Fee Calculation**: Use `vsize` as the denominator for a fee rate

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The path is malformed, or a hash or height is invalid |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No object matches the requested hash, height, or outpoint |
| 500 | Internal error | The node failed to process the request |
