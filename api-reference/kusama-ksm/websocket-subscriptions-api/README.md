---
description: >-
  WebSocket API reference for Kusama subscriptions. Receive real-time updates
  for extrinsics, block heads, runtime versions, and storage changes.
---

# WebSocket Subscriptions API

Pub-sub methods served over the Kusama WebSocket interface only. Each opens a stream: send the subscribe request, receive a subscription id, then receive a notification per update. Cancel with the matching unsubscribe method.

## Subscriptions

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td><code>chain_subscribeNewHeads</code></td><td>Streams new block headers.</td></tr><tr><td><code>chain_subscribeFinalizedHeads</code></td><td>Streams finalized block headers.</td></tr><tr><td><code>state_subscribeStorage</code></td><td>Streams storage changes for keys.</td></tr><tr><td><code>author_submitAndWatchExtrinsic</code></td><td>Submits an extrinsic and streams its status.</td></tr><tr><td><code>chain_subscribeAllHeads</code></td><td>Streams all block headers, including forks.</td></tr><tr><td><code>chain_subscribeRuntimeVersion</code></td><td>Streams runtime version changes.</td></tr><tr><td><code>grandpa_subscribeJustifications</code></td><td>Streams GRANDPA finality justifications.</td></tr><tr><td><code>beefy_subscribeJustifications</code></td><td>Streams BEEFY justifications.</td></tr><tr><td><code>transactionWatch_v1_submitAndWatch</code></td><td>Submits and watches a transaction using the new specification.</td></tr></tbody></table>

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)
