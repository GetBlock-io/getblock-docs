---
description: Learn how to deploy a smart contract on Flare using GetBlock's RPC endpoint.
---

# Deploy a smart contract on Flare

Flare is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. The one difference from an Ethereum deploy is that gas is paid in FLR. This guide covers adding the network to a wallet and deploying a first contract to Flare.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with FLR on Flare for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-flare.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-flare.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with FLR on Flare

## Network Details

| Property        | Value                                                                 |
| --------------- | --------------------------------------------------------------------- |
| Network Name    | Flare Mainnet                                                         |
| RPC URL         | https://shared.eu-central-1.getblock.io//                             |
| Chain ID        | 14 (0xe)                                                              |
| Currency Symbol | FLR                                                                   |
| Block Explorer  | [flare-explorer.flare.network](https://flare-explorer.flare.network/) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Flare is not, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-flare.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `14`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Flare network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Flare gas is paid in FLR.

* **Mainnet** — acquire FLR from an exchange or bridge, then send it to your deployer's address.
* **Testnet (Coston2, chain ID 114)** — claim test C2FLR from the Coston2 faucet; see [dev.flare.network](https://dev.flare.network/) for the current faucet and endpoints.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-flare && cd hello-flare
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Flare";

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
export FLARE_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $FLARE_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Flare's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 14 \
  --verifier blockscout \
  --verifier-url https://flare-explorer.flare.network/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-flare && cd hello-flare
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

const FLARE_CHAIN_ID = 14;

module.exports = {
  solidity: '0.8.24',
  networks: {
    flare: {
      url: process.env.FLARE_RPC_URL,
      chainId: FLARE_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { flare: 'empty' },
    customChains: [
      {
        network: 'flare',
        chainId: FLARE_CHAIN_ID,
        urls: {
          apiURL: 'https://flare-explorer.flare.network/api',
          browserURL: 'https://flare-explorer.flare.network'
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
    string public greeting = "Hello, Flare";

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
export FLARE_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network flare
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network flare <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

{% hint style="info" %}
Flare's distinctive feature is its enshrined data protocols. If your contract consumes price feeds (FTSO) or external/cross-chain attestations (Flare Data Connector), review the Flare periphery contract interfaces in the Flare developer documentation before deploying to mainnet.
{% endhint %}

## After Deploying

* Read contract state through the [eth\_call](/broken/pages/cc516dce1062ba0b50df1a06980154df17d84c4f) method against your GetBlock endpoint.
* Watch contract events with [eth\_getLogs](/broken/pages/6cf1b284f5d5e5dbdcb2318120d3c094163aa6bd).
* Explore your contract and transactions on the [Flare Explorer](https://flare-explorer.flare.network/).
