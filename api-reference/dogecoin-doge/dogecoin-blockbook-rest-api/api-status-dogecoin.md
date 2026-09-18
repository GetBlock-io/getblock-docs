---
description: >-
  Example code for the api/status REST method. Complete guide on how to use the
  api/status REST method in the GetBlock Web3 documentation.
---

# api/status - Dogecoin

This endpoint returns the indexer's sync state and the connected Dogecoin Core node's metadata. It is the first call to make against a new endpoint: it confirms the access token works, identifies the chain, and reports how far the index has caught up to the node.

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
        "coin": "Dogecoin",
        "network": "DOGE",
        "host": "doge-blockbook-ds-host143-mainnet-0",
        "version": "unknown",
        "syncMode": true,
        "initialSync": false,
        "inSync": true,
        "bestHeight": 6378959,
        "lastBlockTime": "2026-09-18T04:31:54.470491915Z",
        "inSyncMempool": true,
        "mempoolSize": 40,
        "decimals": 8,
        "dbSize": 150560920138,
        "hasFiatRates": true
    },
    "backend": {
        "chain": "main",
        "blocks": 6378959,
        "headers": 6378959,
        "bestBlockHash": "7d6f509b16de07844787f97b50778d032eb026a470eb0e0fb5b42a73290e7848",
        "difficulty": "25802469.97465503",
        "sizeOnDisk": 203150546553,
        "version": "1140900",
        "subversion": "/Shibetoshi:1.14.9/",
        "protocolVersion": "70015"
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
| backend.blocks | integer | Best block height known to the Dogecoin Core node |
| backend.subversion | string | Dogecoin Core node user agent |

{% hint style="info" %}
`blockbook.bestHeight` is how far the **index** has processed; `backend.blocks` is how far the **node** has synced. A gap between them means address, UTXO, and balance responses are stale even though the node is current.

The node user agent is `/Shibetoshi:1.14.9/` — Dogecoin Core identifies itself as Shibetoshi, not Dogecoin. `version`, `gitCommit`, and `buildTime` report `unknown` on the shared deployment.
{% endhint %}

## Use Cases

* **Endpoint Verification**: Confirm the access token and base URL work before integrating
* **Sync Gating**: Require `inSync` before trusting a balance or UTXO response
* **Health Monitoring**: Alert when `bestHeight` falls behind `backend.blocks`
* **Diagnostics**: Report indexer and node versions in support requests

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
