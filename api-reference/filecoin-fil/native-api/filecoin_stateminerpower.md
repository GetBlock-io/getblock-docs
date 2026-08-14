# filecoin\_stateminerpower

This method returns the storage power of a miner and the total network power at a tipset. Power determines a miner's chance of winning block rewards.

{% hint style="info" %}
Many state methods take a tipset key as their final parameter: an array of block CIDs identifying the tipset to read. Pass an empty array `[]` to read against the current chain head.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                                             |
| --------- | ------ | -------- | ------------------------------------------------------- |
| address   | string | Yes      | The miner address (f0...)                               |
| tipsetKey | array  | Yes      | Tipset key to read from; empty array for the chain head |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "Filecoin.StateMinerPower", "params": ["f01234", []], "id": "getblock.io"}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://go.getblock.io/<ACCESS-TOKEN>/', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
        jsonrpc: '2.0',
        method: 'Filecoin.StateMinerPower',
        params: ["f01234", []],
        id: 'getblock.io'
    })
});

const data = await response.json();
console.log(data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'Filecoin.StateMinerPower',
        'params': ["f01234", []],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "MinerPower": {
            "RawBytePower": "10995116277760",
            "QualityAdjPower": "10995116277760"
        },
        "TotalPower": {
            "RawBytePower": "27000000000000000000",
            "QualityAdjPower": "27000000000000000000"
        },
        "HasMinPower": true
    }
}
```

## Response Parameters

| Field                      | Type    | Description                                         |
| -------------------------- | ------- | --------------------------------------------------- |
| MinerPower.RawBytePower    | string  | The miner's raw byte storage power                  |
| MinerPower.QualityAdjPower | string  | The miner's quality-adjusted power                  |
| TotalPower                 | object  | Total network raw and quality-adjusted power        |
| HasMinPower                | boolean | Whether the miner meets the minimum power threshold |

## Use Cases

* **Miner Analytics**: Read a storage provider's power
* **Reward Estimation**: Estimate block-reward probability from power share
* **Network Metrics**: Read total network storage power
* **Eligibility Checks**: Confirm a miner meets the minimum power

## Error Handling

| Error Code | Message         | Description                                                 |
| ---------- | --------------- | ----------------------------------------------------------- |
| -32602     | Invalid params  | A parameter is missing or has the wrong type or format      |
| 1          | Actor not found | The address or actor does not exist in the requested tipset |
| -32603     | Internal error  | The node failed to process the request                      |
