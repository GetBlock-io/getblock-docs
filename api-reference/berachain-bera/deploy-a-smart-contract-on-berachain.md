---
description: >-
  Learn how to deploy a smart contract on Berachain using GetBlock's RPC
  endpoint.
---

# Deploy a smart contract on Berachain

Berachain is EVM-identical, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. This guide covers adding the network to a wallet and deploying a first contract to Berachain.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with BERA on Berachain for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-berachain.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-berachain.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with BERA on Berachain

## Network Details

| Property        | Value                                     |
| --------------- | ----------------------------------------- |
| Network Name    | Berachain                                 |
| RPC URL         | https://shared.eu-central-1.getblock.io// |
| Chain ID        | 80094 (0x138de)                           |
| Currency Symbol | BERA                                      |
| Block Explorer  | [berascan.com](https://berascan.com/)     |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Berachain is not, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-berachain.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `80094`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Berachain network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Berachain gas is paid in BERA.

* **Mainnet** — acquire BERA from an exchange or bridge, then send it to your deployer's address.
* **Testnet (Bepolia, chain ID 80069)** — claim test BERA from the Bepolia faucet; see [docs.berachain.com](https://docs.berachain.com/) for the current faucet and endpoints.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-bera && cd hello-bera
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Berachain";

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
export BERA_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $BERA_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Berachain uses a Berascan (Etherscan-compatible) explorer:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 80094 \
  --verifier etherscan \
  --verifier-url https://api.berascan.com/api \
  --etherscan-api-key <BERASCAN_API_KEY>
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-bera && cd hello-bera
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

const BERACHAIN_CHAIN_ID = 80094;

module.exports = {
  solidity: '0.8.24',
  networks: {
    berachain: {
      url: process.env.BERA_RPC_URL,
      chainId: BERACHAIN_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { berachain: process.env.BERASCAN_API_KEY },
    customChains: [
      {
        network: 'berachain',
        chainId: BERACHAIN_CHAIN_ID,
        urls: {
          apiURL: 'https://api.berascan.com/api',
          browserURL: 'https://berascan.com'
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
    string public greeting = "Hello, Berachain";

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
export BERA_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network berachain
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
export BERASCAN_API_KEY=your_berascan_api_key
npx hardhat verify --network berachain <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

{% hint style="info" %}
Berachain's Proof-of-Liquidity means protocols can earn BGT emissions by having their liquidity pools and vaults whitelisted as reward gauges. If your contract is a dApp that provides liquidity, see the Berachain documentation on reward vaults and BGT to integrate with PoL.
{% endhint %}
