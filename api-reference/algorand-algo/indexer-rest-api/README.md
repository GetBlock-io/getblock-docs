---
description: >-
  GetBlock provides fast and reliable access to Algorand nodes via the Indexer
  REST API. Connect to the Algorand network without running your own
  infrastructure.
---

# Indexer REST API

The complete Indexer REST interface for Algorand, generated from the official OpenAPI specification. Endpoints marked _(dedicated)_ are for node administration or diagnostic operations and are not served on GetBlock shared endpoints.

## Endpoints

<table data-search="false"><thead><tr><th>Endpoint</th><th>Method and path</th><th>Description</th></tr></thead><tbody><tr><td>makeHealthCheck</td><td><code>GET /health</code></td><td>Returns 200 if healthy.</td></tr><tr><td>searchForAccounts</td><td><code>GET /v2/accounts</code></td><td>searchForAccounts</td></tr><tr><td>lookupAccountByID</td><td><code>GET /v2/accounts/{account-id}</code></td><td>lookupAccountByID</td></tr><tr><td>lookupAccountAppLocalStates</td><td><code>GET /v2/accounts/{account-id}/apps-local-state</code></td><td>lookupAccountAppLocalStates</td></tr><tr><td>lookupAccountAssets</td><td><code>GET /v2/accounts/{account-id}/assets</code></td><td>lookupAccountAssets</td></tr><tr><td>lookupAccountCreatedApplications</td><td><code>GET /v2/accounts/{account-id}/created-applications</code></td><td>lookupAccountCreatedApplications</td></tr><tr><td>lookupAccountCreatedAssets</td><td><code>GET /v2/accounts/{account-id}/created-assets</code></td><td>lookupAccountCreatedAssets</td></tr><tr><td>lookupAccountTransactions</td><td><code>GET /v2/accounts/{account-id}/transactions</code></td><td>lookupAccountTransactions</td></tr><tr><td>searchForApplications</td><td><code>GET /v2/applications</code></td><td>searchForApplications</td></tr><tr><td>lookupApplicationByID</td><td><code>GET /v2/applications/{application-id}</code></td><td>lookupApplicationByID</td></tr><tr><td>lookupApplicationBoxByIDAndName</td><td><code>GET /v2/applications/{application-id}/box</code></td><td>Get box information for a given application.</td></tr><tr><td>searchForApplicationBoxes</td><td><code>GET /v2/applications/{application-id}/boxes</code></td><td>Get box names for a given application.</td></tr><tr><td>lookupApplicationLogsByID</td><td><code>GET /v2/applications/{application-id}/logs</code></td><td>lookupApplicationLogsByID</td></tr><tr><td>searchForAssets</td><td><code>GET /v2/assets</code></td><td>searchForAssets</td></tr><tr><td>lookupAssetByID</td><td><code>GET /v2/assets/{asset-id}</code></td><td>lookupAssetByID</td></tr><tr><td>lookupAssetBalances</td><td><code>GET /v2/assets/{asset-id}/balances</code></td><td>lookupAssetBalances</td></tr><tr><td>lookupAssetTransactions</td><td><code>GET /v2/assets/{asset-id}/transactions</code></td><td>lookupAssetTransactions</td></tr><tr><td>searchForBlockHeaders</td><td><code>GET /v2/block-headers</code></td><td>searchForBlockHeaders</td></tr><tr><td>lookupBlock</td><td><code>GET /v2/blocks/{round-number}</code></td><td>lookupBlock</td></tr><tr><td>searchForTransactions</td><td><code>GET /v2/transactions</code></td><td>searchForTransactions</td></tr><tr><td>lookupTransaction</td><td><code>GET /v2/transactions/{txid}</code></td><td>lookupTransaction</td></tr></tbody></table>

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)
