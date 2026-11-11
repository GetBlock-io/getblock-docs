---
description: >-
  JSON-RPC API reference for TRON blockchain. Explore method list, request
  examples, and how to connect to GetBlock's TRON RPC endpoints
---

# Tron JSON-RPC API

TRON exposes an Ethereum-compatible JSON-RPC subset over the `/jsonrpc` path, so EVM-oriented tooling can read TRON data and TVM contract state. Every request is an HTTP `POST` with a JSON-RPC 2.0 body: a `method`, `params`, and an `id`. Responses carry the same `id` and a `result` or `error`.

Addresses in this API use TRON's hex (`0x41...`) form, and balances are in SUN. For transaction creation, staking, and TRON-native features, use the [REST API](../tron-rest-api/); TRON's idiomatic SDK, TronWeb, is built on REST rather than this JSON-RPC layer.

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/jsonrpc
```
{% endtab %}

{% tab title="New York, USA" %}
{% code overflow="wrap" %}
```bash
https://shared.us-east-1.getblock.io/<ACCESS_TOKEN/jsonrpc
```
{% endcode %}
{% endtab %}

{% tab title="Singapore, Singapore" %}
{% code overflow="wrap" %}
```bash
https://shared.ap-southeast-1.getblock.io/<ACCESS_TOKEN/jsonrpc
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard.
{% endhint %}

## Chain & Network

| Method              | Description                       |
| ------------------- | --------------------------------- |
| eth\_chainId        | Chain ID (0x2b6653dc = 728126428) |
| eth\_blockNumber    | Latest block number               |
| net\_version        | Network ID (728126428)            |
| web3\_clientVersion | Client software version           |
| web3\_sha3          | Keccak-256 hash of data           |
| eth\_syncing        | Node sync status                  |

## Account & State

| Method            | Description                        |
| ----------------- | ---------------------------------- |
| eth\_getBalance   | TRX balance of an account (in SUN) |
| eth\_getCode      | Contract bytecode at an address    |
| eth\_getStorageAt | Value at a storage slot            |

## Blocks

| Method                                | Description                  |
| ------------------------------------- | ---------------------------- |
| eth\_getBlockByNumber                 | Block by number              |
| eth\_getBlockByHash                   | Block by hash                |
| eth\_getBlockTransactionCountByNumber | Transaction count in a block |

## Transactions

| Method                     | Description                                     |
| -------------------------- | ----------------------------------------------- |
| eth\_getTransactionByHash  | Transaction by hash                             |
| eth\_getTransactionReceipt | Transaction receipt with logs                   |
| buildTransaction           | Build a TRON transaction from an EVM-style call |

## Execution & Logs

| Method           | Description                  |
| ---------------- | ---------------------------- |
| eth\_call        | Read-only contract call      |
| eth\_estimateGas | Estimate Energy (as gas)     |
| eth\_gasPrice    | Energy price (as gas price)  |
| eth\_getLogs     | Event logs matching a filter |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [TRON Official JSON-RPC Reference](https://developers.tron.network/reference/json-rpc-api-overview)
