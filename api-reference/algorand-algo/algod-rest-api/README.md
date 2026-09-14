---
description: >-
  GetBlock provides fast and reliable access to Algorand nodes via the Algod
  REST API. Connect to the Algorand network without running your own
  infrastructure.
---

# Algod REST API

The Algorand node daemon (algod) REST API: live node status, blocks, current account/asset/application state, suggested transaction parameters, transaction submission, and TEAL compilation. Use it for real-time reads and writes.

## Endpoints

<table data-search="false"><thead><tr><th>Endpoint</th><th>Description</th></tr></thead><tbody><tr><td><code>health</code></td><td>Node health check</td></tr><tr><td><code>genesis</code></td><td>Genesis configuration</td></tr><tr><td><code>status</code></td><td>Current node status</td></tr><tr><td><code>block</code></td><td>Block by round</td></tr><tr><td><code>account</code></td><td>Current account state</td></tr><tr><td><code>pending-transactions-by-address</code></td><td>Pending transactions for an account</td></tr><tr><td><code>pending-transaction</code></td><td>Pending transaction information (confirmation)</td></tr><tr><td><code>suggested-params</code></td><td>Suggested transaction parameters</td></tr><tr><td><code>submit-transaction</code></td><td>Submit a signed transaction</td></tr><tr><td><code>application</code></td><td>Application (smart contract) information</td></tr><tr><td><code>asset</code></td><td>Asset (ASA) parameters</td></tr><tr><td><code>supply</code></td><td>Current ledger supply</td></tr><tr><td><code>teal-compile</code></td><td>Compile a TEAL program</td></tr></tbody></table>

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)
