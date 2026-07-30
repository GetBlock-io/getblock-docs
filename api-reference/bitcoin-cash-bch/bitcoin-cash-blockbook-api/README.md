---
description: >-
  GetBlock provides fast and reliable access to Bitcoin Cash nodes via Blockbook
  API. Connect to the Bitcoin Cash network without running your own
  infrastructure.
---

# Bitcoin Cash Blockbook API

Blockbook is an address-indexed and xpub-indexed API for Bitcoin Cash. A standard Bitcoin Cash node tracks unspent outputs but does not organize them by address, so it cannot answer address-level questions on its own. Blockbook, built by Trezor, maintains that index on top of the chain and answers questions about an address or a whole wallet: balances, transaction history, and unspent outputs.

The API is served in two interchangeable forms. A [REST](../bitcoin-cash-blockbook-rest-api/) interface exposes each query as an HTTP path with query parameters, and a JSON-RPC interface exposes the same queries as `bb_`-prefixed methods. Both return the same indexed data.

## What it does

* **Address queries**: Return the balance, transaction history, or unspent outputs of a single address.
* **Wallet queries**: Return a whole wallet from one extended public key or descriptor, with the derived addresses and combined history.
* **Derivation support**: Cover the BIP44, BIP49, BIP84, and BIP86 derivation schemes.
* **Filtering and paging**: Page through large result sets and filter transaction history by block-height range.
* **Fiat rates**: Return current and historical fiat exchange rates for the chain.

## Base Paths

The REST interface is served under an `/api/v2/` path on the Blockbook endpoint, and the JSON-RPC interface is served at the endpoint root.

| Interface | Path                                            |
| --------- | ----------------------------------------------- |
| REST      | `https://go.getblock.io/<ACCESS-TOKEN>/api/v2/` |
| JSON-RPC  | `https://go.getblock.io/<ACCESS-TOKEN>/`        |

{% hint style="info" %}
Blockbook is provisioned as an add-on. Confirm from the GetBlock dashboard that you have chosen the Blockbook add-on.
{% endhint %}

## Available Methods

Each method is available through both the REST and the JSON-RPC interface.

| Method                | Description                                         |
| --------------------- | --------------------------------------------------- |
| bb\_getAddress        | Balance and transaction data for an address         |
| bb\_getXpub           | Wallet-level data for an xpub or descriptor         |
| bb\_getUTXOs          | Unspent outputs for an address, xpub, or descriptor |
| bb\_getBalanceHistory | Aggregated balance history over time                |
| bb\_getTx             | Normalized transaction by id                        |
| bb\_getTxSpecific     | Node-native transaction JSON by id                  |
| bb\_getBlock          | Block by height or hash with its transactions       |
| bb\_getBlockHash      | Block hash at a given height                        |
| bb\_sendTransaction   | Broadcast a signed raw transaction                  |
| bb\_getTickers        | Current or historical fiat rates                    |
| bb\_getTickersList    | Currencies available at a timestamp                 |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Blockbook Documentation](https://github.com/trezor/blockbook/blob/master/docs/api.md)
* [Blockbook Add-on](../../../add-ons/blockbook.md)
* [Blockbook OpenAPI Specification](https://github.com/trezor/blockbook/blob/master/openapi.yaml)
* [Bitcoin Cash (BCH)](../)
