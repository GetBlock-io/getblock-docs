# Cosmos REST API - Cronos

The Cosmos SDK REST (LCD / gRPC-gateway) interface for Cronos: HTTP/JSON module queries for accounts, balances, staking, distribution, and governance, plus transaction broadcast and simulation. Addresses use the bech32 `crc1…` form.

## Methods

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>auth-account</td><td>Account details and sequence</td></tr><tr><td>bank-balances</td><td>All token balances for an address</td></tr><tr><td>bank-supply</td><td>Total supply of all denoms</td></tr><tr><td>staking-validators</td><td>List staking validators</td></tr><tr><td>staking-validator</td><td>A single validator by operator address</td></tr><tr><td>staking-pool</td><td>Bonded and not-bonded token pools</td></tr><tr><td>distribution-rewards</td><td>Outstanding staking rewards for a delegator</td></tr><tr><td>gov-proposals</td><td>List governance proposals</td></tr><tr><td>latest-block</td><td>Latest block (Cosmos base)</td></tr><tr><td>broadcast-tx</td><td>Broadcast a signed transaction</td></tr></tbody></table>

## Support

* Support: support@getblock.io
