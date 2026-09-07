---
tags:
  - kusama
---

# WebSocket Subscriptions API - Kusama

Pub-sub methods served over the Kusama WebSocket interface only. Each opens a stream: send the subscribe request, receive a subscription id, then receive a notification per update. Cancel with the matching unsubscribe method.

## Subscriptions

* [chain\_subscribeNewHeads](/broken/pages/f99f8e840c07896145d0d2e7844d86060d8785bc) — Stream new block headers
* [chain\_subscribeFinalizedHeads](/broken/pages/3f85ee984d27b8a878beee9cfb1a5b275f67eba5) — Stream finalized block headers
* [state\_subscribeStorage](/broken/pages/7b91cec6899f009779ae509ebf2494396ae2afb6) — Stream storage changes for keys
* [author\_submitAndWatchExtrinsic](/broken/pages/5f7a419f0f214a636201191d28e8b280e39120af) — Submit an extrinsic and stream its status

## Support

Support: [support@getblock.io](mailto:support@getblock.io)
