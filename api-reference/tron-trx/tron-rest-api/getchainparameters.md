# getchainparameters

This endpoint returns the current network parameters set by on-chain governance, such as Energy and Bandwidth prices and transaction fees.

## Parameters

This endpoint does not require parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/wallet/getchainparameters' \
--header 'Content-Type: application/json' \
--data-raw '{}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/wallet/getchainparameters',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({})
    }
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/wallet/getchainparameters',
    headers={'Content-Type': 'application/json'},
    json={}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "chainParameter": [
    {
      "key": "getEnergyFee",
      "value": 210
    },
    {
      "key": "getTransactionFee",
      "value": 1000
    },
    {
      "key": "getCreateAccountFee",
      "value": 100000
    }
  ]
}
```

## Response Parameters

| Field                   | Type    | Description                               |
| ----------------------- | ------- | ----------------------------------------- |
| chainParameter          | array   | Network parameters as key and value pairs |
| chainParameter\[].key   | string  | Parameter name, such as getEnergyFee      |
| chainParameter\[].value | integer | Parameter value, in SUN where a price     |

## Use Cases

* **Fee Calculation**: Read the Energy price to compute call costs
* **Cost Modeling**: Read transaction and account-creation fees
* **Governance Tracking**: Detect parameter changes voted on-chain
* **Wallet Configuration**: Configure fee estimates from live parameters

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
