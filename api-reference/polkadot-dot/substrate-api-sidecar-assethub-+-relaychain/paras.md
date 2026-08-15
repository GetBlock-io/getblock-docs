# paras

This endpoint returns the parachains registered on the relay chain and their lifecycle status. It is a relay-chain endpoint.

## Endpoint

```http
GET /paras
```

## Query Parameters

| Parameter | Type   | Required | Description                      |
| --------- | ------ | -------- | -------------------------------- |
| at        | string | No       | Block height or hash to query at |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/paras'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://go.getblock.io/<ACCESS-TOKEN>/paras');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/paras')
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "at": {
    "hash": "0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32",
    "height": "6754362"
  },
  "paras": [
    {
      "paraId": "1000",
      "paraLifecycle": "Parachain"
    },
    {
      "paraId": "2004",
      "paraLifecycle": "Parachain"
    }
  ]
}
```

## Response Fields

| Field                  | Type   | Description                                         |
| ---------------------- | ------ | --------------------------------------------------- |
| paras                  | array  | Registered parachains                               |
| paras\[].paraId        | string | The parachain ID                                    |
| paras\[].paraLifecycle | string | Lifecycle state (Parachain, Parathread, Onboarding) |

## Use Cases

* **Parachain Explorers**: List registered parachains
* **Ecosystem Tools**: Track parachain lifecycle states
* **Monitoring**: Detect onboarding or offboarding paras
* **Analytics**: Map the parachain landscape
