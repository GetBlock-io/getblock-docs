---
description: >-
  Example code for the api/v2/tx-specific REST method. Complete guide on how to
  use the api/v2/tx-specific REST method in the GetBlock Web3 documentation.
---

# api/v2/tx-specific - Bitcoin Cash

This endpoint returns the transaction exactly as the Bitcoin Cash node reports it, in the node's own JSON shape rather than the indexer's normalized schema.

## Parameters

| Parameter | Type   | Location | Required | Description        |
| --------- | ------ | -------- | -------- | ------------------ |
| txid      | string | path     | Yes      | The transaction id |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "txid": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642",
    "hash": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642",
    "version": 1,
    "size": 226,
    "locktime": 0,
    "vin": [
        {
            "txid": "780791bb2d5a8ccda4b5a707967a8e15b412814852c58c77299e85579bb65587",
            "vout": 1,
            "sequence": 4294967295
        }
    ],
    "vout": [
        {
            "value": 0.09407625,
            "n": 0,
            "scriptPubKey": {
                "type": "pubkeyhash",
                "addresses": [
                    "bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a"
                ]
            }
        }
    ],
    "blockhash": "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc",
    "confirmations": 1197,
    "time": 1617180599,
    "blocktime": 1617180599
}
```

## Response Parameters

| Field         | Type    | Description                                                          |
| ------------- | ------- | -------------------------------------------------------------------- |
| txid          | string  | The transaction id                                                   |
| version       | integer | Transaction version                                                  |
| vin           | array   | Raw inputs as reported by the node                                   |
| vout          | array   | Raw outputs with node-native scriptPubKey and BCH-denominated values |
| confirmations | integer | Number of confirmations                                              |
| blocktime     | integer | Block time as a Unix timestamp                                       |

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
