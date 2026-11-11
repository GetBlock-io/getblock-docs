---
description: >-
  gRPC API reference for TRON blockchain. Explore service list, request
  examples, and how to connect to GetBlock's TRON RPC endpoints
---

# Tron gRPC API

TRON nodes expose the same operation set as the REST API over gRPC, using Protocol Buffers. gRPC is the transport used internally by java-tron and by performance-sensitive backends; the request and response messages mirror the REST endpoints one-to-one.

{% hint style="info" %}
The GetBlock TRON reference documents the REST (HTTP) and JSON-RPC interfaces in full. This page describes the gRPC transport and how its services map to the documented REST endpoints, so a gRPC integration can reuse the same request and response shapes.
{% endhint %}

## Services

TRON's protobuf API is organized into two services that correspond to the two node types:

| Service          | Node     | Corresponds to                                                  |
| ---------------- | -------- | --------------------------------------------------------------- |
| `Wallet`         | Fullnode | The `/wallet/{method}` REST endpoints (full API)                |
| `WalletSolidity` | Solidity | The `/walletsolidity/{method}` REST endpoints (confirmed reads) |

## Method mapping

Each gRPC method maps to a REST endpoint of the same name; the protobuf request and response messages carry the same fields documented on the REST pages.

| gRPC method (Wallet service) | REST endpoint                     | Reference                                                                          |
| ---------------------------- | --------------------------------- | ---------------------------------------------------------------------------------- |
| `GetAccount`                 | `/wallet/getaccount`              | [getaccount](../tron-rest-api/wallet-getaccount-tron.md)                           |
| `GetNowBlock`                | `/wallet/getnowblock`             | [getnowblock](../tron-rest-api/wallet-getnowblock-tron.md)                         |
| `GetBlockByNum`              | `/wallet/getblockbynum`           | [getblockbynum](../tron-rest-api/wallet-getblockbynum-tron.md)                     |
| `GetTransactionById`         | `/wallet/gettransactionbyid`      | [gettransactionbyid](../tron-rest-api/wallet-gettransactionbyid-tron.md)           |
| `TriggerConstantContract`    | `/wallet/triggerconstantcontract` | [triggerconstantcontract](../tron-rest-api/wallet-triggerconstantcontract-tron.md) |
| `TriggerContract`            | `/wallet/triggersmartcontract`    | [triggersmartcontract](../tron-rest-api/wallet-triggersmartcontract-tron.md)       |
| `BroadcastTransaction`       | `/wallet/broadcasttransaction`    | [broadcasttransaction](../tron-rest-api/wallet-broadcasttransaction-tron.md)       |
| `FreezeBalanceV2`            | `/wallet/freezebalancev2`         | [freezebalancev2](../tron-rest-api/wallet-freezebalancev2-tron.md)                 |

The protobuf service and message definitions are maintained in the java-tron repository (`api/api.proto`, `core/*.proto`). Use those definitions to generate a gRPC client, then send requests to the GetBlock TRON endpoint.

## Reading the field shapes

Because the gRPC messages and the REST JSON bodies carry the same fields, the parameter and response tables on the REST pages are also the reference for gRPC request and response fields. For request and response detail, see the corresponding endpoint under the [REST API](../tron-rest-api/).

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [TRON Protobuf definitions (java-tron)](https://github.com/tronprotocol/java-tron)
* [REST API](../tron-rest-api/)
