---
description: >-
  Example code for the api/status REST method. Complete guide on how to use the
  api/status REST method in the GetBlock Web3 documentation.
---

# api/status - Zcash

This endpoint returns the indexer's sync state and the connected Zebra node's metadata. It is the first call to make against a new endpoint: it confirms the access token works, identifies the chain, and reports how far the index has caught up to the node.

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
        "coin": "Zcash",
        "network": "ZEC",
        "host": "zec-blockbook-ax52-host4-mainnet-0",
        "version": "unknown",
        "syncMode": true,
        "initialSync": false,
        "inSync": true,
        "bestHeight": 3487817,
        "lastBlockTime": "2026-09-18T15:08:15.389593237Z",
        "inSyncMempool": true,
        "mempoolSize": 1,
        "decimals": 8,
        "dbSize": 32822310284,
        "hasFiatRates": true
    },
    "backend": {
        "chain": "main",
        "blocks": 3487820,
        "headers": 3487820,
        "bestBlockHash": "000000000043481a95bbacf7cffc56bb0bcfa9af9f2f147bb2f5ca9a6adca9f2",
        "difficulty": "290179647.9149114",
        "sizeOnDisk": 278526085121,
        "version": "zebra",
        "consensus": {
            "chaintip": "37a5165b",
            "nextblock": "37a5165b"
        }
    }
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| blockbook.inSync | boolean | True when the index has caught up to the backend's best block |
| blockbook.bestHeight | integer | Height of the best block the indexer has processed |
| blockbook.mempoolSize | integer | Number of transactions in the indexed mempool |
| backend.blocks | integer | Best block height known to the Zebra node |
| backend.version | string | Backend implementation. Reported as `zebra`, with no version number |
| backend.consensus.chaintip | string | Consensus branch ID in force at the chain tip |
| backend.consensus.nextblock | string | Consensus branch ID that will apply to the next block |

{% hint style="info" %}
Zcash is the only Blockbook chain here that reports a `consensus` object. Its values are consensus branch IDs, which change at each network upgrade, and every transaction must be signed for the branch in force. When `nextblock` differs from `chaintip`, a network upgrade activates at the next block and signing software must switch branch ID.

`backend.version` reports `zebra` rather than a version number. Use the JSON-RPC `getinfo` method to read the exact node version.
{% endhint %}

## Use Cases

* **Endpoint Verification**: Confirm the access token and base URL work before integrating
* **Sync Gating**: Require `inSync` before trusting a balance or UTXO response
* **Upgrade Detection**: Watch `consensus.nextblock` differ from `chaintip` ahead of a network upgrade
* **Health Monitoring**: Alert when `bestHeight` falls behind `backend.blocks`

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
