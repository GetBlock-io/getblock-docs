---
description: Learn how to deploy a smart contract on Chiliz using GetBlock's RPC endpoint.
---

# Deploy a smart contract on Chiliz

Chiliz Chain is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. The one difference from an Ethereum deploy is that gas is paid in CHZ. This guide covers adding the network to a wallet and deploying a first contract to Chiliz.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with CHZ on Chiliz for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-chiliz.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-chiliz.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with CHZ on Chiliz

## Network Details

| Property        | Value                                     |
| --------------- | ----------------------------------------- |
| Network Name    | Chiliz Chain                              |
| RPC URL         | https://shared.eu-central-1.getblock.io// |
| Chain ID        | 88888 (0x15b38)                           |
| Currency Symbol | CHZ                                       |
| Block Explorer  | [chiliscan.com](https://chiliscan.com/)   |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Chiliz is not, so it must be added manually — follow the steps below:

{% stepper %}
{% step %}
### Open your wallet

Open MetaMask (or your EVM wallet of choice).
{% endstep %}

{% step %}
### Add a custom network

Open the network selector and choose **Add a custom network**.
{% endstep %}

{% step %}
### Enter the network details

Enter the values from the [Network Details](deploy-a-smart-contract-on-chiliz.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `88888`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Chiliz network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Chiliz gas is paid in CHZ.

* **Mainnet** — acquire CHZ from an exchange or bridge, then send it to your deployer's address.
* **Testnet (Spicy, chain ID 88882)** — claim test CHZ from the Spicy faucet; see [docs.chiliz.com](https://docs.chiliz.com/) for the current faucet and endpoints.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-chiliz && cd hello-chiliz
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Chiliz";

    function setGreeting(string calldata greeting_) external {
        greeting = greeting_;
    }
}
```
{% endcode %}
{% endstep %}

{% step %}
### Deploy

```bash
export CHZ_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $CHZ_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

ChiliScan is an Etherscan-compatible explorer:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 88888 \
  --verifier etherscan \
  --verifier-url https://chiliscan.com/api \
  --etherscan-api-key <CHILISCAN_API_KEY>
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-chiliz && cd hello-chiliz
npm init --yes
npm install --save-dev hardhat
npx hardhat init
```
{% endstep %}

{% step %}
### Configure the network

{% code title="hardhat.config.js" %}
```javascript
require('@nomicfoundation/hardhat-toolbox');

const CHILIZ_CHAIN_ID = 88888;

module.exports = {
  solidity: '0.8.24',
  networks: {
    chiliz: {
      url: process.env.CHZ_RPC_URL,
      chainId: CHILIZ_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { chiliz: process.env.CHILISCAN_API_KEY },
    customChains: [
      {
        network: 'chiliz',
        chainId: CHILIZ_CHAIN_ID,
        urls: {
          apiURL: 'https://chiliscan.com/api',
          browserURL: 'https://chiliscan.com'
        }
      }
    ]
  }
};
```
{% endcode %}
{% endstep %}

{% step %}
### Write the contract

{% code title="contracts/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Chiliz";

    function setGreeting(string calldata greeting_) external {
        greeting = greeting_;
    }
}
```
{% endcode %}
{% endstep %}

{% step %}
### Deploy

```bash
export CHZ_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network chiliz
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
export CHILISCAN_API_KEY=your_chiliscan_api_key
npx hardhat verify --network chiliz <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

{% hint style="info" %}
Chiliz is designed for Fan Tokens and sports-fan applications. If you are issuing a Fan Token or building on the Socios ecosystem, review the Chiliz token standards and listing requirements in the Chiliz documentation before deploying to mainnet.
{% endhint %}
