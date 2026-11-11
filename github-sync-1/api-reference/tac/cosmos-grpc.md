---
description: >-
  GetBlock provides fast and reliable access to TAC nodes via the Cosmos gRPC
  API. Connect to the TAC network without running your own infrastructure.
---

# Cosmos gRPC

The Cosmos SDK gRPC interface for TAC: the same module query set as the Cosmos REST API, exposed as typed Protocol Buffers services over HTTP/2. Examples use `grpcurl`. Addresses use the bech32 `tac1…` form; amounts are in the base denomination.

## Endpoint

{% code overflow="wrap" %}
```bash
shared.eu-central-1.getblock.io:443   (TLS, access token passed as request metadata)
```
{% endcode %}

{% hint style="info" %}
Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard. gRPC connects to a `host:port` target over TLS, with the access token supplied as request metadata rather than in the URL path. Confirm the exact gRPC target and the access-token metadata key in the GetBlock dashboard. On the SPB testnet, use the Frankfurt target.
{% endhint %}

## Available Services

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>auth Account</td><td>Account details and sequence</td></tr><tr><td>bank Balance</td><td>Balance of one denom for an address</td></tr><tr><td>bank AllBalances</td><td>All balances for an address</td></tr><tr><td>bank TotalSupply</td><td>Total supply of all denoms</td></tr><tr><td>staking Validators</td><td>List staking validators</td></tr><tr><td>staking DelegatorDelegations</td><td>A delegator's delegations</td></tr><tr><td>distribution DelegationTotalRewards</td><td>Outstanding rewards for a delegator</td></tr><tr><td>gov Proposals</td><td>List governance proposals</td></tr><tr><td>tx BroadcastTx</td><td>Broadcast a signed transaction</td></tr></tbody></table>

## Example

List the services exposed by the node, then query a bank balance:

{% code overflow="wrap" %}
```bash
# List available gRPC services (reflection)
grpcurl -H "x-api-key: <ACCESS-TOKEN>" \
  shared.eu-central-1.getblock.io:443 list

# All balances for an address
grpcurl -H "x-api-key: <ACCESS-TOKEN>" \
  -d '{"address":"tac1ql8dru5dgm6gtg3hcdq9lds3q3fnk3s6k9h8f0"}' \
  shared.eu-central-1.getblock.io:443 \
  cosmos.bank.v1beta1.Query/AllBalances
```
{% endcode %}

{% hint style="info" %}
The metadata key for the access token (`x-api-key` above) is illustrative — confirm the exact header the GetBlock gRPC gateway expects in the dashboard.
{% endhint %}

## Response

```json
{
  "balances": [
    { "denom": "atac", "amount": "150000000000000000000" }
  ],
  "pagination": { "total": "1" }
}
```

## Use Cases

* **Typed backends**: Use generated protobuf clients for production services
* **Same query set as REST**: Prefer gRPC for performance; REST for quick HTTP/JSON access
* **Transaction broadcast**: Submit signed transactions via `tx BroadcastTx`

## See Also

* [TAC overview](./)
* [Cosmos REST API - TAC](cosmos-rest/) — the same query set over HTTP/JSON
* [Cosmos SDK gRPC documentation](https://docs.cosmos.network/sdk/latest/api-reference/grpc)
