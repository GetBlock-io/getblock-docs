---
hidden: true
noIndex: true
icon: circle
---

# Querying Web3 data using Covalent API

Covalent is pleased to partner with industry-leading service providers such as GetBlock. Covalent provides a unified API to unlock one billion possibilities and bring full transparency and visibility to assets across all blockchain networks.

Simply put, the API allows you to pull detailed, granular and historical blockchain transaction data from multiple blockchains with no code. The Covalent API allows you to create entirely new applications or augment existing ones without configuring or maintaining infrastructure.

## API Key Required

Sign up at [https://www.covalenthq.com/platform/](https://www.covalenthq.com/platform/) to get your API key

## API Documentation

The Covalent API docs reference ([https://www.covalenthq.com/docs/api](https://www.covalenthq.com/docs/api)) provides all the detailed information on supported blockchain networks, available API endpoints, and even enables users to make all API requests directly from the browser.

Key points to note about the Covalent API are:

* The Covalent API is RESTful;
* A Covalent API key must be passed as a query parameter with all requests using: key=API\_KEY;
* The default return format for all endpoints is JSON.CSV format is supported for some endpoints;
* The refresh rate is classified as:\
  real-time: 30s or 2 blocks;\
  batch: 10m or 40 blocks;
* The primary query parameter to switch between blockchain networks is the chain\_id. So for example, to fetch all the token balances (including NFTs) of a wallet address on Ethereum and Binance Smart Chain, requests are made to the balances\_v2 endpoint:

https://api.covalenthq.com/v1/{chain\_id}/address/{address}/balances\_v2/

where chain\_id is 1 for Ethereum mainnet and 56 for BSC mainnet. The Covalent API supports all major blockchain networks. See [https://www.covalenthq.com/docs/networks](https://www.covalenthq.com/docs/networks/?utm_source=website\&utm_medium=info\&utm_campaign=getblock) for the full list including chain\_id values.

## Learning Resources:

Covalent provides a comprehensive learning resource in the form of Tutorials, How-to Guides and Case Studies at [https://www.covalenthq.com/docs/learn](https://www.covalenthq.com/docs/learn/?utm_source=website\&utm_medium=info\&utm_campaign=getblock)

## Use Cases:

Access to rich, historical blockchain data unlocks a whole host of use cases, many of which are still unknown and coming to light with the rapid pace of innovation in this space. Some current use cases include:

* Multi-chain wallets to present balances, transactions, ROI, portfolio values as well as analytics on token concentrations, volumes and distribution
* NFT dashboards showing price trends, liquidity and ROI of collectibles
* Tax compliance tools
* DEX analytics and transparency dashboards
* DAO governance and analytics dashboards To see what developers have built using the Covalent API, check out our Project Showcase: [https://www.covalenthq.com/docs/project-showcase](https://www.covalenthq.com/docs/project-showcase/?utm_source=website\&utm_medium=info\&utm_campaign=getblock)

## Developer Tools:

These tools are complementary to the [Covalent API](https://www.covalenthq.com/docs/api/?utm_source=website\&utm_medium=info\&utm_campaign=getblock) and provide additional features and functionality to extract and analyze complex blockchain data. The page below lists tools which have been built by the community to provide value to users of the Covalent API.

Note: These tools are NOT maintained by the Covalent Team and users should do their own due diligence in evaluating these tools before using them in their projects. [https://www.covalenthq.com/docs/tools/community](https://www.covalenthq.com/docs/tools/community/?utm_source=website\&utm_medium=info\&utm_campaign=getblock)
