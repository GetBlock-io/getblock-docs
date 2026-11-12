---
description: >-
  Sui Indexed Archive gives you the full history of Sui mainnet from genesis,
  indexed and served over the same gRPC calls as live data on your GetBlock  
  Dedicated Node.
---

# Sui Indexed Archive

Sui Indexed Archive is a managed archive of Sui mainnet history, from the genesis checkpoint on May 3, 2023 to the present. GetBlock indexes that history and runs it alongside your Sui Dedicated Node, so a transaction from 2023 is returned by the same gRPC call you use for one from a minute ago.

You don't have to host an archive, run a database cluster, or backfill anything. The index is ready on the day your node is deployed.

{% hint style="info" %}
Sui Indexed Archive is available on Sui Dedicated Nodes. To order it, see [How to activate it](sui-indexed-archive.md#how-to-activate-it) or go to the [Sui Indexed Archive page](https://getblock.io/sui-indexed-archive/).
{% endhint %}

## What it does

A regular Sui full node prunes old data and keeps only recent epochs. Sui Indexed Archive keeps everything and indexes it by transaction digest, object version, and checkpoint sequence number, so each lookup is a single request.

You can query:

| Data                           | How you look it up       | gRPC method                                                                                                                                                                                                |
| ------------------------------ | ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Any transaction since genesis  | By transaction digest    | [`LedgerService/GetTransaction`](../api-reference/sui-sui/ledgerservice-gettransaction-sui.md), [`LedgerService/BatchGetTransactions`](../api-reference/sui-sui/ledgerservice-batchgettransactions-sui.md) |
| Any object at any past version | By object ID and version | [`LedgerService/GetObject`](../api-reference/sui-sui/ledgerservice-getobject-sui.md)                                                                                                                       |
| Any checkpoint since genesis   | By sequence number       | [`LedgerService/GetCheckpoint`](../api-reference/sui-sui/ledgerservice-getcheckpoint-sui.md)                                                                                                               |

Archive reads use Sui's standard `LedgerService` gRPC methods. They go to the same endpoint as live reads on your Dedicated Node, so your application needs no second URL and no fallback logic for old data. If your code already calls `LedgerService`, it works without changes.

{% hint style="warning" %}
Sui Indexed Archive is served over gRPC. Sui's JSON-RPC interface is deprecated, so historical queries are not offered over JSON-RPC. GraphQL access to the archive is not generally available yet; [contact us](https://getblock.io/contact/) if you need it.
{% endhint %}

## Who needs it

Sui Indexed Archive is for teams whose product depends on data older than a pruned node keeps:

* **DEXes and aggregators:** rebuild past pool states for price charts, trade history, and LP dashboards.
* **Wallets:** show every transaction a user has signed since 2023, including those a pruned full node no longer has.
* **Security and forensics teams:** pull the exact transactions and object versions needed for an AML case or an audit.
* **Backtesters and market makers:** replay strategies against real Sui history, and reconcile fills and inventory down to the checkpoint.
* **Banks and fintechs:** back client statements and regulator requests with on-chain records.
* **Protocol and indexer teams:** backfill a custom indexer by reading checkpoints from genesis over gRPC, in place of downloading raw checkpoint files.

If you only need occasional historical lookups, access to the Sui archive on a Shared Node may be enough. It serves the same history over gRPC without the index.

## Why it's better

Sui Indexed Archive compares with the other ways of reading Sui history as follows:

| Option                                   | What you have to do                                                                                                          | With Sui Indexed Archive                                                  |
| ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| Pruned full node                         | Nothing, but only recent epochs are available                                                                                | Full history from genesis                                                 |
| Raw checkpoint archive                   | Download and parse checkpoint files to find one old transaction                                                              | One gRPC call per lookup                                                  |
| Unpruned archive node                    | Store the full history on your own disks and sync from genesis                                                               | GetBlock stores and maintains it                                          |
| Self-hosted Archival Service on Bigtable | Run a Google Cloud Bigtable cluster (Mysten recommends at least 7 nodes) and wait for a backfill of about 4 days on 30 nodes | Index is ready at deployment, with no Bigtable or Google Cloud dependency |

Beyond that, you get:

* **One endpoint for live and historical data.** Archive and live reads share your Dedicated Node endpoint.
* **A flat monthly price.** Archive reads are not billed per call and there are no RPS caps, so the cost stays the same as your traffic grows. Pricing starts from $2,800 per month; see the [Sui Indexed Archive page](https://getblock.io/sui-indexed-archive/) for current pricing and a cost comparison with self-hosting.
* **A choice of location.** Deploy in Frankfurt, New York, or Singapore, or on your own hardware as an on-premises cluster that GetBlock deploys and manages.
* **Dedicated Node guarantees.** Dedicated Node contracts include a 99.99% uptime SLA, and GetBlock holds a SOC 2 Type II attestation (the report is available under NDA).

## How to activate it

Sui Indexed Archive is set up by the GetBlock team as part of a Sui Dedicated Node. There is no self-service toggle.

{% stepper %}
{% step %}
#### Send a request

**Request Sui Dedicated Archive** by contacting [the GetBlock team](https://getblock.io/contact/).
{% endstep %}

{% step %}
#### Share your requirements

Tell us your expected query volume and the region you want: Frankfurt, New York, Singapore, or on-premises.
{% endstep %}

{% step %}
#### Review the proposal

GetBlock replies with a node configuration, the SOC 2 Type II report, and a start date.
{% endstep %}

{% step %}
#### Start querying

Once your Dedicated Node is deployed, the archive is already indexed. Call the `LedgerService` methods on your node's endpoint as you would for live data. See the [Sui API reference](../api-reference/sui-sui/) for request and response formats.
{% endstep %}
{% endstepper %}

## Related pages

* [Sui API reference](../api-reference/sui-sui/)
* [Sui Indexed Archive overview and pricing](https://getblock.io/sui-indexed-archive/)
* [Sui nodes on GetBlock](https://getblock.io/nodes/sui/)
