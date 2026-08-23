---
description: >-
  Example code for the api/v2/tx-specific REST method. Complete guide on how to
  use the api/v2/tx-specific REST method in the GetBlock Web3 documentation.
---

# api/v2/tx-specific - Bitcoin

This endpoint returns the transaction exactly as the Bitcoin node reports it, in the node's own JSON shape rather than the indexer's normalized schema.

## Parameters

| Parameter | Type   | Location | Required | Description        |
| --------- | ------ | -------- | -------- | ------------------ |
| txid      | string | path     | Yes      | The transaction id |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
    "hash": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
    "version": 1,
    "size": 226,
    "locktime": 0,
    "vin": [
        {
            "txid": "6a3e1f2b8c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d6e7f8091",
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
                    "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq"
                ]
            }
        }
    ],
    "blockhash": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428",
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
| vout          | array   | Raw outputs with node-native scriptPubKey and BTC-denominated values |
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
