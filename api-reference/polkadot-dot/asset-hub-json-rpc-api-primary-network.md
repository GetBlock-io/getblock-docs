# Asset Hub JSON-RPC API (Primary Network)

Asset Hub is a system parachain and the primary network for fungible assets and NFTs on Polkadot. It runs the standard Substrate runtime plus the asset pallets (Assets, Foreign Assets, Pool Assets, and NFTs), and it is positioned to hold balances and tokens on behalf of the wider network.

Because Asset Hub is a Substrate chain, its JSON-RPC interface is the **same method set documented in the** [**Polkadot JSON-RPC API (Relaychain)**](polkadot-json-rpc-api-relaychain/) reference: the `chain_`, `state_`, `system_`, `payment_`, `author_`, `grandpa_`, `childstate_`, and `mmr_` namespaces, called the same way. On GetBlock's unified endpoint, JSON-RPC requests reach Asset Hub as the primary network. What differs is the **data** those methods return — Asset Hub's blocks, state, and metadata include the asset pallets.

## Base URL

{% tabs %}
{% tab title="HTTPS" %}
```bash
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="WebSocket" %}
```bash
wss://go.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

## Method Reference

The full JSON-RPC method reference applies to Asset Hub unchanged. See the [Relaychain method index](polkadot-json-rpc-api-relaychain/) for `chain_`, `state_`, `system_`, `payment_`, `grandpa_`, `childstate_`, and `mmr_` methods, each with parameters, responses, and a Polkadot.js example.

Two properties confirm the chain a JSON-RPC endpoint is serving:

* `system_chain` returns the chain name.
* `system_properties` returns `ss58Format` 0, `tokenDecimals` 10, and `tokenSymbol` DOT — Asset Hub uses DOT as its native token.

## Reading Asset Hub Assets

Asset Hub's distinctive data lives in the asset pallets. It can be read using the same JSON-RPC state methods or, more conveniently, via the [Substrate API Sidecar](substrate-api-sidecar-assethub-+-relaychain/) REST endpoints.

### Asset details and metadata

Read an asset's supply, owner, and metadata (name, symbol, decimals):

{% tabs %}
{% tab title="Sidecar (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/pallets/assets/1984/asset-info'
```
{% endcode %}
{% endtab %}

{% tab title="JSON-RPC (state_getStorage)" %}
{% code overflow="wrap" %}
```bash
# Storage key = twox128("Assets") ++ twox128("Metadata") ++ blake2_128_concat(assetId)
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "state_getStorage", "params": ["0x..."], "id": "getblock.io"}'
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Account asset balances

Read an account's balances across Asset Hub assets:

{% code overflow="wrap" %}
```bash
curl --location 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/accounts/15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5/asset-balances?assets[]=1984'
```
{% endcode %}

### Enumerating assets

Use `state_getKeysPaged` against the `Assets.Asset` storage prefix to page through every asset ID, then resolve each with `state_getStorage` or the Sidecar `asset-info` endpoint.

{% hint style="info" %}
For NFTs, the same pattern applies against the `Nfts` (and legacy `Uniques`) pallet storage. Collection and item metadata are read with `state_getStorage`, or through the Sidecar pallet endpoints.
{% endhint %}

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [**Polkadot JSON-RPC API (Relaychain)**](polkadot-json-rpc-api-relaychain/)&#x20;
* [Substrate API Sidecar](substrate-api-sidecar-assethub-+-relaychain/)
* [Asset Hub overview](https://wiki.polkadot.network/docs/learn-assets)
