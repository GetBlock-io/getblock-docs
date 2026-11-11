---
description: >-
  Example code for the api/status REST method. Complete guide on how to use the
  api/status REST method in the GetBlock Web3 documentation.
---

# api/status - Bitcoin

This endpoint returns the indexer's sync state and the connected backend node's metadata. It is the first call to make against a new endpoint: it confirms the access token works, identifies the chain, and reports how far the index has caught up to the node.

The same payload is served at `/api/`, `/api/v2`, and `/api/v2/`. All four paths are equivalent; `/api/status` is the canonical one.

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
        "host": "btc-blockbook-ax51-host145-mainnet-0",
        "version": "unknown",
        "gitCommit": "unknown",
        "buildTime": "unknown",
        "syncMode": true,
        "initialSync": false,
        "inSync": true,
        "bestHeight": 967487,
        "lastBlockTime": "2026-09-18T02:39:02.680814461Z",
        "inSyncMempool": true,
        "lastMempoolTime": "2026-09-18T02:46:27.090875355Z",
        "mempoolSize": 67990,
        "decimals": 8,
        "dbSize": 619514712165,
        "hasFiatRates": true,
        "currentFiatRatesTime": "2026-09-18T02:46:02.841087873Z",
        "about": "Blockbook - blockchain indexer for Trezor Suite https://trezor.io/trezor-suite. Do not use for any other purpose."
    },
    "backend": {
        "chain": "main",
        "blocks": 967487,
        "headers": 967487,
        "bestBlockHash": "000000000000000000017cfd38e8af73159da4d9ab4c3f82ae183659f7b09793",
        "difficulty": "127450789715843.1",
        "sizeOnDisk": 877048058747,
        "version": "310100",
        "subversion": "/Satoshi:31.1.0/",
        "protocolVersion": "70016"
    }
}
```

{% hint style="info" %}
`version`, `gitCommit`, and `buildTime` report `unknown` on GetBlock's shared Blockbook deployment. Use `backend.version` and `backend.subversion` to identify the node, and do not gate logic on the indexer build fields.
{% endhint %}

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
