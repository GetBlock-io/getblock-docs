---
description: >-
  Example code for the api/status REST method. Complete guide on how to use the
  api/status REST method in the GetBlock Web3 documentation.
---

# api/status - Bitcoin

This endpoint returns the indexer's sync state and the connected backend node's metadata. It is the first call to make against a new endpoint: it confirms the access token works, identifies the chain, and reports how far the index has caught up to the node.

The same payload is served at `/api/` on full public interfaces.

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
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/status'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
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
        "coin": "Bitcoin",
        "network": "BTC",
        "host": "backend5",
        "version": "0.5.1",
        "gitCommit": "a0960c8e",
        "buildTime": "2024-08-08T12:32:50+00:00",
        "syncMode": true,
        "initialSync": false,
        "inSync": true,
        "bestHeight": 860730,
        "lastBlockTime": "2024-09-10T08:19:04.471017534Z",
        "inSyncMempool": true,
        "lastMempoolTime": "2024-09-10T08:42:39.38871351Z",
        "mempoolSize": 232021,
        "decimals": 8,
        "dbSize": 761283489075,
        "hasFiatRates": true,
        "currentFiatRatesTime": "2024-09-10T08:42:00.898792419Z",
        "historicalFiatRatesTime": "2024-09-10T00:00:00Z",
        "about": "Blockbook - blockchain indexer for Trezor Suite."
    },
    "backend": {
        "chain": "main",
        "blocks": 860730,
        "headers": 860730,
        "bestBlockHash": "00000000000000000000effeb0c4460480e6a347deab95332c63007a68646ee5",
        "difficulty": "89471664776970.77",
        "sizeOnDisk": 681584532221,
        "version": "270100",
        "subversion": "/Satoshi:27.1.0/",
        "protocolVersion": "70016"
    }
}
```

## Response Parameters

| Field                     | Type    | Description                                                           |
| ------------------------- | ------- | ----------------------------------------------------------------------- |
| blockbook.coin            | string  | Coin the indexer serves                                               |
| blockbook.network         | string  | Network ticker                                                        |
| blockbook.version         | string  | Blockbook indexer version                                             |
| blockbook.inSync          | boolean | True when the index has caught up to the backend's best block         |
| blockbook.initialSync     | boolean | True while the index is still building for the first time             |
| blockbook.bestHeight      | integer | Height of the best block the indexer has processed                    |
| blockbook.lastBlockTime   | string  | Timestamp of the most recently indexed block                          |
| blockbook.inSyncMempool   | boolean | True when the mempool index is current                                |
| blockbook.mempoolSize     | integer | Number of transactions in the indexed mempool                         |
| blockbook.decimals        | integer | Decimal places in the coin's base unit                                |
| blockbook.hasFiatRates    | boolean | True when fiat rate data is available on this instance                |
| backend.chain             | string  | Backend chain name, `main` for mainnet                                |
| backend.blocks            | integer | Best block height known to the backend node                           |
| backend.headers           | integer | Best header height known to the backend node                          |
| backend.bestBlockHash     | string  | Hash of the backend node's best block                                 |
| backend.version           | string  | Backend node version                                                  |
| backend.subversion        | string  | Backend node user agent                                               |

## Use Cases

* **Endpoint Verification**: Confirm the access token and base URL work before integrating
* **Chain Assertion**: Check `coin` and `backend.chain` to confirm the endpoint serves the expected network
* **Sync Gating**: Require `inSync` before trusting a balance or UTXO response
* **Health Monitoring**: Alert when `bestHeight` falls behind `backend.blocks`
* **Diagnostics**: Report indexer and node versions in support requests

{% hint style="info" %}
`blockbook.bestHeight` is how far the **index** has processed; `backend.blocks` is how far the **node** has synced. A gap between the two means address, UTXO, and balance responses are stale even though the node itself is current. Gate settlement logic on `inSync` rather than on the endpoint merely responding.
{% endhint %}

## Error Handling

| HTTP Status | Message        | Description                               |
| ----------- | -------------- | ----------------------------------------- |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN           |
| 500         | Internal error | The indexer failed to report its status   |
