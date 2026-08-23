# bb\_gettxspecific bitcoin

This method returns the transaction exactly as the Bitcoin node reports it, in the node's own JSON shape rather than the indexer's normalized schema.

## Parameters

| Parameter | Type   | Required | Description        |
| --------- | ------ | -------- | ------------------ |
| txid      | string | Yes      | The transaction ID |

## Request

{% tabs %}
{% tab title="cURL (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b'
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
    "method": "bb_getTxSpecific",
    "params": [
        "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b"
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx-specific/4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b');
const data = await response.json();
console.log(data);
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
  "version": 2,
  "size": 226,
  "vsize": 144,
  "locktime": 0,
  "vin": [
    {
      "txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
      "vout": 0,
      "sequence": 4294967295
    }
  ],
  "vout": [
    {
      "value": 6.2499,
      "n": 0,
      "scriptPubKey": {
        "type": "witness_v0_keyhash",
        "address": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq"
      }
    }
  ],
  "blocktime": 1706886000,
  "confirmations": 152
}
```

## Response Parameters

| Field         | Type    | Description                                     |
| ------------- | ------- | ----------------------------------------------- |
| txid          | string  | Transaction ID                                  |
| vin           | array   | Node-native inputs                              |
| vout          | array   | Node-native outputs with BTC-denominated values |
| confirmations | integer | Number of confirmations                         |

## Use Cases

* **Node-Native Data**: Read the transaction in Bitcoin Core's format
* **Script Analysis**: Inspect node-native scriptPubKey data
* **Compatibility**: Feed tools expecting Bitcoin Core JSON
* **Verification**: Cross-check normalized data against node output

## Error Handling

| HTTP Status | Message      | Description                               |
| ----------- | ------------ | ----------------------------------------- |
| 400         | Bad request  | A path or query parameter is malformed    |
| 403         | Forbidden    | Missing or invalid ACCESS-TOKEN           |
| 404         | Not found    | The requested resource does not exist     |
| 500         | Server error | The indexer failed to process the request |
