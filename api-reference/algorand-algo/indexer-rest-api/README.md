---
description: >-
  GetBlock provides fast and reliable access to Algorand nodes via the Indexer
  REST API. Connect to the Algorand network without running your own
  infrastructure.
---

# Indexer REST API

The Algorand Indexer REST API: archival, filtered, paginated search over transactions, accounts, assets (ASAs), applications, and blocks. Use it for history and analytics that algod does not retain.

## Endpoints

The Indexer REST API includes these endpoints:

<table data-search="false"><thead><tr><th>Endpoint</th><th>Description</th></tr></thead><tbody><tr><td><code>health</code></td><td>Indexer health and round</td></tr><tr><td><code>account</code></td><td>Account state at a round</td></tr><tr><td><code>account-transactions</code></td><td>Transaction history for an account</td></tr><tr><td><code>account-assets</code></td><td>Assets held by an account</td></tr><tr><td><code>search-transactions</code></td><td>Search transactions</td></tr><tr><td><code>transaction</code></td><td>Transaction by id</td></tr><tr><td><code>search-assets</code></td><td>Search assets (ASAs)</td></tr><tr><td><code>asset</code></td><td>Asset by id (indexer)</td></tr><tr><td><code>asset-balances</code></td><td>Holders of an asset</td></tr><tr><td><code>asset-transactions</code></td><td>Transactions for an asset</td></tr><tr><td><code>search-applications</code></td><td>Search applications</td></tr><tr><td><code>application</code></td><td>Application by id (indexer)</td></tr><tr><td><code>block</code></td><td>Block by round (indexer)</td></tr></tbody></table>

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)
