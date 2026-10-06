---
description: Learn how to deploy a smart contract on Core using GetBlock's RPC endpoint.
---

# Deploy a smart contract on Core

Core is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. The one difference from an Ethereum deploy is that gas is paid in CORE. This guide covers adding the network to a wallet and deploying a first contract to Core.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with CORE on Core for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-core.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-core.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with CORE on Core

## Network Details

| Property        | Value                                         |
| --------------- | --------------------------------------------- |
| Network Name    | Core Blockchain                               |
| RPC URL         | https://shared.eu-central-1.getblock.io//     |
| Chain ID        | 1116 (0x45c)                                  |
| Currency Symbol | CORE                                          |
| Block Explorer  | [scan.coredao.org](https://scan.coredao.org/) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Core is not, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-core.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `1116`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Core network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Core gas is paid in CORE.

* **Mainnet** — acquire CORE from an exchange or bridge, then send it to your deployer's address.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-core && cd hello-core
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Core";

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
export CORE_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $CORE_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Core Scan is an Etherscan-compatible explorer:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 1116 \
  --verifier etherscan \
  --verifier-url https://openapi.coredao.org/api \
  --etherscan-api-key <CORESCAN_API_KEY>
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-core && cd hello-core
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

const CORE_CHAIN_ID = 1116;

module.exports = {
  solidity: '0.8.24',
  networks: {
    core: {
      url: process.env.CORE_RPC_URL,
      chainId: CORE_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { core: process.env.CORESCAN_API_KEY },
    customChains: [
      {
        network: 'core',
        chainId: CORE_CHAIN_ID,
        urls: {
          apiURL: 'https://openapi.coredao.org/api',
          browserURL: 'https://scan.coredao.org'
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
    string public greeting = "Hello, Core";

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
export CORE_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network core
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
export CORESCAN_API_KEY=your_corescan_api_key
npx hardhat verify --network core <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

{% hint style="info" %}
Core's distinctive feature is non-custodial Bitcoin staking and Satoshi Plus. If your application integrates BTC staking or dual staking (CORE + BTC), review the Core staking contracts and the stCORE liquid-staking documentation before deploying to mainnet.
{% endhint %}

## After Deploying

* Read contract state through the [eth\_call](/broken/pages/3890116fe0f31d7e7bc09b55e9f4ef45c1255c0c) method against your GetBlock endpoint.
* Watch contract events with [eth\_getLogs](/broken/pages/fd2a6713b76763c0fbafd2a99835f4cdac23806c).
* Explore your contract and transactions on [Core Scan](https://scan.coredao.org/).
