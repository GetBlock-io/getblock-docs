---
description: >-
  This guide covers adding the network to a browser wallet, then deploying a
  simple contract using Foundry and Hardhat — pick whichever you prefer.
---

# Deploy a smart contract on Arbitrum Nova

Arbitrum Nova is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint. This guide covers adding the network to a wallet and deploying a first contract to Arbitrum Nova.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with ETH on Arbitrum Nova for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-arbitrum-nova.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-arbitrum-nova.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with ETH on Arbitrum Nova

## Network Details

| Property        | Value                                         |
| --------------- | --------------------------------------------- |
| Network Name    | Arbitrum Nova                                 |
| RPC URL         | https://shared.eu-central-1.getblock.io//     |
| Chain ID        | 42170 (0xa4ba)                                |
| Currency Symbol | ETH                                           |
| Block Explorer  | [nova.arbiscan.io](https://nova.arbiscan.io/) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Arbitrum Nova is not, so it must be added manually — use the button below or the manual steps that follow.

To add the network manually:

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
### Enter network details

Enter the values from the [Network Details](deploy-a-smart-contract-on-arbitrum-nova.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `42170`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Arbitrum Nova network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Arbitrum Nova gas is paid in ETH bridged from Ethereum.

* **Mainnet** — bridge ETH from Ethereum L1 using the official [Arbitrum Bridge](https://bridge.arbitrum.io/) and select Arbitrum Nova as the destination. Bridged ETH becomes spendable on Arbitrum Nova for gas.
* **Testing** — Arbitrum Nova has no dedicated public testnet; contract logic is typically validated on Arbitrum Sepolia or a local Nitro dev node before deploying to Nova mainnet.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-nova && cd hello-nova
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Arbitrum Nova";

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
export NOVA_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $NOVA_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Arbitrum Nova uses an Arbiscan (Etherscan-compatible) explorer:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 42170 \
  --verifier etherscan \
  --verifier-url https://api-nova.arbiscan.io/api \
  --etherscan-api-key <ARBISCAN_API_KEY>
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-nova && cd hello-nova
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

const NOVA_CHAIN_ID = 42170;

module.exports = {
  solidity: '0.8.24',
  networks: {
    nova: {
      url: process.env.NOVA_RPC_URL,
      chainId: NOVA_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { nova: process.env.ARBISCAN_API_KEY },
    customChains: [
      {
        network: 'nova',
        chainId: NOVA_CHAIN_ID,
        urls: {
          apiURL: 'https://api-nova.arbiscan.io/api',
          browserURL: 'https://nova.arbiscan.io'
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
    string public greeting = "Hello, Arbitrum Nova";

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
export NOVA_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network nova
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
export ARBISCAN_API_KEY=your_arbiscan_api_key
npx hardhat verify --network nova <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

{% hint style="info" %}
For accurate gas estimation on Arbitrum Nova, query the NodeInterface precompile (virtual address `0xC8`) via `gasEstimateComponents`, which separates the L2 execution gas from the L1 data component. A plain `eth_gasPrice` reflects only the L2 component.
{% endhint %}
