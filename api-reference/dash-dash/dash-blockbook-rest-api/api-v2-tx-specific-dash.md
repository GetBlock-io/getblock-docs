---
description: >-
  Example code for the api/v2/tx-specific REST method. Complete guide on how to
  use the api/v2/tx-specific REST method in the GetBlock Web3 documentation.
---

# api/v2/tx-specific - Dash

This endpoint returns the transaction exactly as the Dash node reports it, in the node's own JSON shape rather than the indexer's normalized schema.

## Parameters

| Parameter | Type   | Location | Required | Description        |
| --------- | ------ | -------- | -------- | ------------------ |
| txid      | string | path     | Yes      | The transaction id |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/eb0351c64cde2c42bcdf10b11a1ad44bb63631bb6487de70e78b90c2aa57137c'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/eb0351c64cde2c42bcdf10b11a1ad44bb63631bb6487de70e78b90c2aa57137c'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/eb0351c64cde2c42bcdf10b11a1ad44bb63631bb6487de70e78b90c2aa57137c')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "txid": "eb0351c64cde2c42bcdf10b11a1ad44bb63631bb6487de70e78b90c2aa57137c",
    "version": 2,
    "type": 0,
    "size": 1278,
    "locktime": 0,
    "vin": [
        {
            "txid": "3d427a54eac26a3eb54ffa7ea41172043afd64963a810e5d1119e5cb503b6184",
            "vout": 2,
            "sequence": 4294967295
        }
    ],
    "vout": [
        {
            "value": 0.01000010,
            "n": 0,
            "scriptPubKey": {
                "type": "pubkeyhash",
                "addresses": [
                    "XhNtSggaqVecdFxSFeDftR6GZk8D7Ms3JE"
                ]
            }
        }
    ],
    "blockhash": "00000000000000007409ab5be18f66b1f68795ff81d3b3b1d592574fae07c372",
    "height": 2540600,
    "confirmations": 74,
    "time": 1789687365,
    "blocktime": 1789687365,
    "instantlock": true,
    "instantlock_internal": true,
    "chainlock": true
}
```

## Response Parameters

| Field                | Type    | Description                                                                       |
| -------------------- | ------- | ----------------------------------------------------------------------------------- |
| txid                 | string  | The transaction id                                                                |
| version              | integer | Transaction version                                                               |
| type                 | integer | Dash special-transaction type. 0 for a standard payment                           |
| size                 | integer | Serialized size in bytes                                                          |
| locktime             | integer | Transaction lock time                                                             |
| vin                  | array   | Raw inputs as reported by the node                                                |
| vout                 | array   | Raw outputs with node-native scriptPubKey and values denominated in DASH          |
| blockhash            | string  | Hash of the containing block                                                      |
| height               | integer | Height of the containing block                                                    |
| confirmations        | integer | Number of confirmations                                                           |
| time                 | integer | Block time as a Unix timestamp                                                    |
| blocktime            | integer | Block time as a Unix timestamp                                                    |
| instantlock          | boolean | True when the transaction is locked by InstantSend                                |
| instantlock_internal | boolean | True when this node itself observed the InstantSend lock, rather than inferring it |
| chainlock            | boolean | True when the containing block is protected by a ChainLock                        |

{% hint style="info" %}
`instantlock`, `instantlock_internal`, `chainlock`, and `type` are Dash-specific and have no equivalent in the normalized [api/v2/tx](api-v2-tx-dash.md) schema, which is shared across all coins. This endpoint is the only way to read them.

For payment flows they matter more than confirmation count: `instantlock: true` means the inputs are locked by the masternode quorum and the transaction cannot be double-spent, which is normally reached within seconds. `chainlock: true` means the block itself is final and cannot be reorganized. Either is a stronger settlement signal on Dash than waiting a fixed number of blocks.
{% endhint %}

Values in `vout` are denominated in DASH as decimal numbers, not in duffs as integer strings. This is the node's own encoding; the normalized [api/v2/tx](api-v2-tx-dash.md) endpoint reports duffs instead.

## Use Cases

* **Node Parity**: Read the node's exact transaction JSON for compatibility
* **Script Inspection**: Access native scriptPubKey fields not in the normalized schema
* **Migration**: Match output from a direct node integration during a switch
* **Debugging**: Compare normalized and node-native views of a transaction

## Error Handling

| HTTP Status | Message        | Description                                               |
| ----------- | -------------- | --------------------------------------------------------- |
| 400         | Bad request    | The transaction id is not a valid 64-character hex string |
| 404         | Not found      | No transaction with the requested id is indexed           |
| 500         | Internal error | The indexer failed to read the transaction                |
