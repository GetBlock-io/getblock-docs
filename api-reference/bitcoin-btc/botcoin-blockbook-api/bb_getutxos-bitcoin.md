# bb\_getutxos bitcoin

This method returns the unspent transaction outputs for an address, extended public key, or descriptor. These outputs are the inputs available when constructing a spending transaction.

## Parameters

| Parameter     | Type    | Required | Description                                 |
| ------------- | ------- | -------- | ------------------------------------------- |
| addressOrXpub | string  | Yes      | Address, extended public key, or descriptor |
| confirmed     | boolean | No       | Return only confirmed UTXOs when true       |

## Request

{% tabs %}
{% tab title="cURL (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq?confirmed=true'
```
{% endcode %}
{% endtab %}

{% tab title="cURL (JSON-RPC)" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "bb_getUTXOs",
    "params": [
        "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq",
        {
            "confirmed": true
        }
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq?confirmed=true');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq?confirmed=true')
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
[
  {
    "txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
    "vout": 0,
    "value": "50000000",
    "height": 830000,
    "confirmations": 152,
    "address": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq",
    "path": "m/84'/0'/0'/0/0"
  }
]
```

## Response Parameters

| Field         | Type    | Description                  |
| ------------- | ------- | ---------------------------- |
| txid          | string  | Transaction ID of the output |
| vout          | integer | Output index                 |
| value         | string  | Output value in satoshis     |
| confirmations | integer | Number of confirmations      |
| height        | integer | Block height of the output   |

## Use Cases

* **Coin Selection**: Read spendable outputs for a transaction
* **Balance Building**: Sum UTXOs to compute a balance
* **Wallets**: Build transactions from available outputs
* **Consolidation**: Identify outputs to consolidate

## Error Handling

| HTTP Status | Message      | Description                               |
| ----------- | ------------ | ----------------------------------------- |
| 400         | Bad request  | A path or query parameter is malformed    |
| 403         | Forbidden    | Missing or invalid ACCESS-TOKEN           |
| 404         | Not found    | The requested resource does not exist     |
| 500         | Server error | The indexer failed to process the request |
