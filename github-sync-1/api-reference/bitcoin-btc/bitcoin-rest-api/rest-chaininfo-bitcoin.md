---
description: >-
  Example code for the rest/chaininfo REST endpoint. Complete guide on how to use the
  rest/chaininfo REST endpoint in the GetBlock Web3 documentation.
---

# rest/chaininfo - Bitcoin

This endpoint returns the node's view of the chain: height, best block, difficulty, and verification progress. It is the cheapest call to confirm an endpoint works and that the node is in sync.

## Parameters

This endpoint takes no parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/chaininfo.json'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/chaininfo.json'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/chaininfo.json')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "chain": "main",
    "blocks": 967816,
    "headers": 967816,
    "bestblockhash": "000000000000000000019c481c042c0625abe1dc04fe26795e2e24b152791e5f",
    "bits": "17021ec5",
    "target": "000000000000000000021ec50000000000000000000000000000000000000000",
    "difficulty": 132757073449487.5,
    "time": 1789899076,
    "mediantime": 1789892696,
    "verificationprogress": 1,
    "initialblockdownload": false,
    "chainwork": "000000000000000000000000000000000000000148e422b78a65626de4ddb156",
    "size_on_disk": 877666977499,
    "pruned": false,
    "warnings": ""
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| chain | string | Network name: `main`, `test`, `signet`, or `regtest` |
| blocks | numeric | Height of the most-work fully-validated chain |
| headers | numeric | Height of the best header chain. Equals `blocks` when fully synced |
| bestblockhash | string | Hash of the current chain tip |
| difficulty | numeric | Current proof-of-work difficulty |
| target | string | Current proof-of-work target |
| mediantime | numeric | Median time of the last 11 blocks |
| verificationprogress | numeric | Estimate of verification progress, 0 to 1 |
| initialblockdownload | boolean | True while the node is still in initial block download |
| chainwork | string | Total cumulative work on the chain, hex-encoded |
| size_on_disk | numeric | Space the block files occupy, in bytes |
| pruned | boolean | True when the node is running pruned |

{% hint style="info" %}
This is Bitcoin Core's own REST interface, served by the node. It is a read-only view of the chain: there is no address index, so it cannot answer "what is the balance of this address". Those queries belong on the [Blockbook add-on](../bitcoin-blockbook-rest-api/).
{% endhint %}

## Use Cases

* **Endpoint Verification**: Confirm the token and base URL work before integrating
* **Sync Gating**: Require `blocks` to equal `headers` before trusting chain reads
* **Network Assertion**: Check `chain` is `main` before treating data as mainnet
* **Dashboards**: Surface height, difficulty, and verification progress

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The path is malformed, or a hash or height is invalid |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No object matches the requested hash, height, or outpoint |
| 500 | Internal error | The node failed to process the request |
