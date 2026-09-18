---
description: >-
  Example code for the api/status REST method. Complete guide on how to use the
  api/status REST method in the GetBlock Web3 documentation.
---

# api/status - Litecoin

This endpoint returns the indexer's sync state and the connected Litecoin Core node's metadata. It is the first call to make against a new endpoint: it confirms the access token works, identifies the chain, and reports how far the index has caught up to the node.

The same payload is served at `/api/`, `/api/v2`, and `/api/v2/`.

## Parameters

This endpoint takes no parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/status'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/status'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/status')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "blockbook": {
        "coin": "Litecoin",
        "network": "LTC",
        "host": "ltc-blockbook-ax51-host113-mainnet-0",
        "version": "unknown",
        "syncMode": true,
        "initialSync": false,
        "inSync": true,
        "bestHeight": 3179816,
        "lastBlockTime": "2026-09-18T03:19:17.759471524Z",
        "inSyncMempool": true,
        "mempoolSize": 46,
        "decimals": 8,
        "dbSize": 205280833844,
        "hasFiatRates": true
    },
    "backend": {
        "chain": "main",
        "blocks": 3179816,
        "headers": 3179816,
        "bestBlockHash": "3e469b185937924138e5ab9b4fda4901dc2ad5507c7190fb56968c0654a0a1ac",
        "difficulty": "88389131.60278592",
        "sizeOnDisk": 265457548078,
        "version": "210508",
        "subversion": "/LitecoinCore:0.21.5.8/",
        "protocolVersion": "70017"
    }
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| blockbook.coin | string | Coin the indexer serves |
| blockbook.inSync | boolean | True when the index has caught up to the backend's best block |
| blockbook.bestHeight | integer | Height of the best block the indexer has processed |
| blockbook.mempoolSize | integer | Number of transactions in the indexed mempool |
| backend.blocks | integer | Best block height known to the Litecoin Core node |
| backend.subversion | string | Litecoin Core node user agent |

{% hint style="info" %}
`blockbook.bestHeight` is how far the **index** has processed; `backend.blocks` is how far the **node** has synced. A gap between them means address, UTXO, and balance responses are stale even though the node is current. `version`, `gitCommit`, and `buildTime` report `unknown` on the shared deployment, so identify the node through `backend.subversion` instead.
{% endhint %}

## Use Cases

* **Endpoint Verification**: Confirm the access token and base URL work before integrating
* **Sync Gating**: Require `inSync` before trusting a balance or UTXO response
* **Health Monitoring**: Alert when `bestHeight` falls behind `backend.blocks`
* **Diagnostics**: Report indexer and node versions in support requests

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 500 | Internal error | The indexer failed to report its status |
