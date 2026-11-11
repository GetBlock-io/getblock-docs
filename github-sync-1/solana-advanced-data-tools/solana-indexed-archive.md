---
description: >-
  Solana indexed archive data that allows users to efficiently query any
  historical information.
---

# Solana Indexed Archive

Solana Indexed Archive is a software development kit(SDK) that provides an indexed data layer, enabling instant access to the complete Solana blockchain history and real-time data through a single, high-performance API. Built on SQD Network infrastructure, it eliminates the need to work with raw Solana data, run archive nodes, or maintain complex indexing logic.

_Interested in building on Solana with Indexed Archive?_ [_Reach us_](https://getblock.io/contact) _for more information._

### The Indexed Archive Solution

* **Complete historical index:** Every block, transaction, instruction, log, and account update already parsed, normalized, and ready for queries
* **Unified access layer:** Single endpoint for both historical queries and live subscriptions
* **Developer-friendly SDK:** Typed queries, easy integration, no low-level Solana structures\
  Zero operational overhead: GetBlock operates all infrastructure
* &#x20;**Instant availability:** Full Solana history accessible immediately, no setup or backfilling

Using the SDK, you can easily build ETL pipelines that extract and transform on-chain data without handling raw Solana events, decoding account structures, or maintaining complex indexing logic. All blockchain data has already been extracted, indexed, and is immediately ready to use. Your only task is to define the query and specify the required filter.

### Technical Architecture

#### Layer 1: SQD Network - Decentralized Indexing

SQD Network continuously processes Solana's blockstream in a decentralized manner:

* Block processing: Every Solana block parsed immediately upon production
* Data extraction: Transactions, instructions, logs, account updates extracted
* Normalization: Raw data decoded and structured following standard schemas
* Enrichment: Relationships between transactions, accounts, and programs established
* Distributed storage: Indexed data stored across decentralized SQD infrastructure

#### Layer 2: GetBlock Portal - High-Performance Access Layer

GetBlock deploys and maintains a global network of Portal instances—high-performance gateways that:

* Provide unified interface: Single API endpoint for historical data and real-time subscriptions
* Ensure low latency: Geographically distributed clusters for fast global access
* Guarantee high availability: Redundant instances with automatic failover
* Handle scale: Optimized for large workloads and deep historical scans
* Abstract complexity: Clean API hides underlying distributed storage complexity

### What the Indexed Archive Includes

1. Blocks
   1. Block hash, slot number, parent slot
   2. Block time (Unix timestamp)
   3. Leader (validator) identity
   4. Transaction count
   5. Total compute units consumed
2. Transactions
   1. Signature (unique identifier)
   2. Fee payer address
   3. Success/failure status
   4. Error messages (if failed)
   5. Fee paid (lamports)
   6. Compute units used
   7. List of program invocations
3. Instructions
   1. Program ID (which program was called)
   2. Instruction index (order within transaction)
   3. Instruction data (decoded where possible)
   4. Accounts array (all accounts involved)
   5. Nested/inner instructions
4. Logs
   1. Program log messages
   2. Associated transaction signature
   3. Log level and content
   4. Timestamp
5. Account Updates
   1. Account public key
   2. Pre-transaction balance (lamports)
   3. Post-transaction balance (lamports)
   4. Owner program (pre and post)
   5. Data changes (what was modified)
   6. Rent epoch information

### Use Cases

#### Analytics and Trading

1. Deep historical analysis and backtesting
2. Real-time liquidity and TVL monitoring
3. Pattern recognition and signal generation

#### dApps and Wallets

1. Full transaction and asset history for any user
2. Instant UI updates based on on-chain events
3. Aggregation of data across DeFi and NFT protocols

#### Infrastructure Providers and Enterprises

1. Reliable data backbone for internal analytics
2. Foundation for B2B data services
3. No need to maintain validators or archive RPC setups

***

For consultation on optimal deployment architecture for your specific use case, contact [GetBlock support team](https://getblock.io/contact/).&#x20;
