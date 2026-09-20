---
description: >-
  Example code for the rest/deploymentinfo REST endpoint. Complete guide on how to use the
  rest/deploymentinfo REST endpoint in the GetBlock Web3 documentation.
---

# rest/deploymentinfo - Bitcoin

This endpoint reports the activation state of each consensus soft fork the node knows about, as of the chain tip or a given block.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| blockhash | string | path | No | Report deployment state as of this block instead of the tip |
| format | string | path | Yes | Response format: `json` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/deploymentinfo.json'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/deploymentinfo.json'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/deploymentinfo.json')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "hash": "000000000000000000019c481c042c0625abe1dc04fe26795e2e24b152791e5f",
    "height": 967816,
    "script_flags": [
        "CHECKLOCKTIMEVERIFY",
        "CHECKSEQUENCEVERIFY",
        "DERSIG",
        "NULLDUMMY",
        "P2SH",
        "TAPROOT",
        "WITNESS"
    ],
    "deployments": {
        "segwit": {
            "type": "buried",
            "active": true,
            "height": 481824
        },
        "taproot": {
            "type": "bip9",
            "height": 709632,
            "active": true,
            "bip9": {
                "start_time": 1619222400,
                "timeout": 1628640000,
                "min_activation_height": 709632,
                "status": "active",
                "since": 709632,
                "status_next": "active"
            }
        }
    }
}
```

Only `segwit` and `taproot` are shown; the node also reports `bip34`, `bip65`, `bip66`, and `csv`.

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| hash | string | Hash of the block the report is made against |
| height | numeric | Height of that block |
| deployments | object | One entry per soft fork, keyed by name |
| deployments[].type | string | `buried` for forks locked in by height, `bip9` for version-bits deployments |
| deployments[].active | boolean | True when the fork is active at this block |
| deployments[].height | numeric | Activation height |
| deployments[].bip9 | object | Version-bits detail for `bip9` deployments, including `status` and `since` |

{% hint style="info" %}
`buried` deployments were activated long ago and are hardcoded by height. `bip9` deployments carry the full version-bits record, including the signalling window and the height at which the status last changed.
{% endhint %}

## Use Cases

* **Capability Checks**: Confirm Taproot or SegWit is active before relying on it
* **Historical Queries**: Pass a block hash to see what was active at that point
* **Node Comparison**: Verify two nodes agree on consensus rules

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The path is malformed, or a hash or height is invalid |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No object matches the requested hash, height, or outpoint |
| 500 | Internal error | The node failed to process the request |
