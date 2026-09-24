---
description: >-
  GetBlock provides fast and reliable access to Avail data availability methods
  via JSON-RPC API. Connect to the Avail network without running your own
  infrastructure.
---

# Data Availability API

Avail's Kate RPC exposes the data-availability layer: the dimensions of a block's KZG-committed data matrix, its rows and cell proofs for data-availability sampling, and inclusion proofs that a submitted data blob is part of a block's data root. These methods are what make Avail a data-availability layer — rollups and light clients use them to verify that data was published without downloading entire blocks.

## Methods

| Method                | Description                               |
| --------------------- | ----------------------------------------- |
| `kate_blockLength`    | Data matrix dimensions for a block        |
| `kate_queryRows`      | Data matrix rows for a block              |
| `kate_queryProof`     | KZG proofs for data cells                 |
| `kate_queryDataProof` | Inclusion proof for a submitted data blob |

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)
