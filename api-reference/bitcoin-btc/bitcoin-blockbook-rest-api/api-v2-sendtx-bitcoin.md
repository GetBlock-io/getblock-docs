# api v2 sendtx bitcoin

This endpoint broadcasts a signed, serialized transaction to the Bitcoin network through the backend node and returns the transaction id on acceptance. The raw hex is sent in the request body.

## Parameters

| Parameter | Type   | Location | Required | Description                                              |
| --------- | ------ | -------- | -------- | -------------------------------------------------------- |
| hex       | string | body     | Yes      | The raw signed transaction hex, sent as the request body |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/sendtx/' \
--header 'Content-Type: text/plain' \
--data-raw '01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a10100000017160014...signed...raw...tx...hex...00000000'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/sendtx/',
    {
        method: 'POST',
        headers: { 'Content-Type': 'text/plain' },
        body: '01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a10100000017160014...signed...raw...tx...hex...00000000'
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/sendtx/',
    headers={'Content-Type': 'text/plain'},
    data='01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a10100000017160014...signed...raw...tx...hex...00000000'
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "result": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b"
}
```

## Response Parameters

| Field  | Type   | Description                                    |
| ------ | ------ | ---------------------------------------------- |
| result | string | The transaction id of the accepted transaction |

## Use Cases

* **Payment Broadcast**: Submit a signed transaction through the REST endpoint
* **Wallet Backends**: Broadcast transactions built and signed client-side
* **Unified Integration**: Broadcast through the same base URL used for queries
* **Retry Flows**: Resubmit a dropped transaction from stored raw hex

## Error Handling

| HTTP Status | Message              | Description                                    |
| ----------- | -------------------- | ---------------------------------------------- |
| 400         | Bad request          | The transaction hex could not be decoded       |
| 400         | Transaction rejected | The transaction failed node mempool acceptance |
| 500         | Internal error       | The node failed to broadcast the transaction   |
