# api v2 tx bitcoin

This endpoint returns a normalized transaction by its id, with inputs, outputs, addresses, and confirmation data in the indexer's unified schema.

## Parameters

| Parameter | Type    | Location | Required | Description                                                  |
| --------- | ------- | -------- | -------- | ------------------------------------------------------------ |
| txid      | string  | path     | Yes      | The transaction id                                           |
| spending  | boolean | query    | No       | Include spending transaction data for outputs when available |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx/4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx/4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx/4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
    "version": 1,
    "vin": [
        {
            "txid": "6a3e1f2b8c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d6e7f8091",
            "vout": 1,
            "sequence": 4294967295,
            "n": 0,
            "addresses": [
                "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq"
            ],
            "isAddress": true,
            "value": "12345970"
        }
    ],
    "vout": [
        {
            "value": "9407625",
            "n": 0,
            "spent": false,
            "addresses": [
                "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq"
            ],
            "isAddress": true,
            "type": "pubkeyhash"
        }
    ],
    "blockHash": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428",
    "blockHeight": 830000,
    "confirmations": 1197,
    "blockTime": 1617180599,
    "size": 226,
    "value": "9407625",
    "valueIn": "12345970",
    "fees": "2938345"
}
```

## Response Parameters

| Field         | Type    | Description                                                  |
| ------------- | ------- | ------------------------------------------------------------ |
| txid          | string  | The transaction id                                           |
| vin           | array   | Transaction inputs with addresses and values                 |
| vout          | array   | Transaction outputs with addresses, values, and spent status |
| blockHeight   | integer | Block height, or -1 when unconfirmed                         |
| confirmations | integer | Number of confirmations                                      |
| value         | string  | Total output value in satoshis                               |
| fees          | string  | Transaction fee in satoshis                                  |

## Use Cases

* **Transaction Views**: Render a normalized transaction in a wallet or explorer
* **Payment Confirmation**: Read confirmations to confirm a received payment
* **Fee Inspection**: Read the computed fee for a transaction
* **Address Mapping**: Read input and output addresses without decoding raw hex
* **Spend Tracking**: Check whether outputs are spent with the spending flag

## Error Handling

| HTTP Status | Message        | Description                                               |
| ----------- | -------------- | --------------------------------------------------------- |
| 400         | Bad request    | The transaction id is not a valid 64-character hex string |
| 404         | Not found      | No transaction with the requested id is indexed           |
| 500         | Internal error | The indexer failed to read the transaction                |
