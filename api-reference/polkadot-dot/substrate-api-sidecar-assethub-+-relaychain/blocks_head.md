# blocks\_head

This endpoint returns the most recent block, including its extrinsics, events grouped by phase, and author. The finalized head can be requested with the finalized query parameter.

{% hint style="info" %}
On GetBlock's unified endpoint, Asset Hub is the default network. To call this endpoint against the Relaychain, add an `/rc` prefix to the path (for example, `/rc/blocks/head`).
{% endhint %}

## Endpoint

```http
GET /blocks/head
```

## Query Parameters

| Parameter     | Type    | Required | Description                                                 |
| ------------- | ------- | -------- | ----------------------------------------------------------- |
| finalized     | boolean | No       | Return the latest finalized block instead of the best block |
| eventDocs     | boolean | No       | Include event documentation                                 |
| extrinsicDocs | boolean | No       | Include extrinsic documentation                             |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/blocks/head'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://go.getblock.io/<ACCESS-TOKEN>/blocks/head');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/blocks/head')
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "number": "6754362",
  "hash": "0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32",
  "parentHash": "0x570e3f417a41646d9b978bf2ac3d68be48bb0f73082825f438af58a37cfe0ef8",
  "stateRoot": "0x176c67eca385c24403ba774550ff75dcfa652d6d6cf2d2ecbccf56e513db601c",
  "extrinsicsRoot": "0xada254ef8321e28a6667ad75659be6464944174bd5540667da94590a0a4a596f",
  "authorId": "15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5",
  "extrinsics": [],
  "onInitialize": {
    "events": []
  },
  "onFinalize": {
    "events": []
  },
  "finalized": true
}
```

## Response Fields

| Field      | Type    | Description                          |
| ---------- | ------- | ------------------------------------ |
| number     | string  | Block number                         |
| hash       | string  | Block hash                           |
| authorId   | string  | Block author (validator)             |
| extrinsics | array   | Decoded extrinsics with their events |
| finalized  | boolean | Whether the block is finalized       |

## Use Cases

* **Block Explorers**: Render the latest block with decoded extrinsics
* **Indexing**: Ingest blocks without decoding SCALE manually
* **Event Monitoring**: Watch events emitted per block
* **Finality Tracking**: Follow the finalized head
