# Deploy a Smart Contract on Blast

Blast is EVM-equivalent, so contracts deploy using the standard Solidity toolchain — Foundry, Hardhat, or Remix — with a GetBlock endpoint. This guide covers adding the network to a wallet and deploying a first contract to Blast.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with ETH on Blast for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-blast.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-blast.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with ETH on Blast

## Network Details

| Property        | Value                                     |
| --------------- | ----------------------------------------- |
| Network Name    | Blast Mainnet                             |
| RPC URL         | https://shared.eu-central-1.getblock.io// |
| Chain ID        | 81457 (0x13e31)                           |
| Currency Symbol | ETH                                       |
| Block Explorer  | [blastscan.io](https://blastscan.io/)     |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Blast is not, so it must be added manually — use the button below or the manual steps that follow.

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-blast.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `81457`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Blast network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Blast gas is paid in ETH that has been bridged from Ethereum.

* **Mainnet** — bridge ETH from Ethereum L1 using the official [Blast Bridge](https://blast.io/bridge). Bridged ETH becomes spendable on Blast for gas and begins auto-rebasing.
* **Testnet (Blast Sepolia, chain ID 168587773)** — obtain Ethereum Sepolia ETH from a public Sepolia faucet, then bridge it to Blast Sepolia with the [Blast Bridge](https://blast.io/bridge) in testnet mode.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-blast && cd hello-blast
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Blast";

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
export BLAST_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $BLAST_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Blast uses a Blastscan (Etherscan-compatible) explorer:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 81457 \
  --verifier etherscan \
  --verifier-url https://api.blastscan.io/api \
  --etherscan-api-key <BLASTSCAN_API_KEY>
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-blast && cd hello-blast
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

const BLAST_CHAIN_ID = 81457;

module.exports = {
  solidity: '0.8.24',
  networks: {
    blast: {
      url: process.env.BLAST_RPC_URL,
      chainId: BLAST_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { blast: process.env.BLASTSCAN_API_KEY },
    customChains: [
      {
        network: 'blast',
        chainId: BLAST_CHAIN_ID,
        urls: {
          apiURL: 'https://api.blastscan.io/api',
          browserURL: 'https://blastscan.io'
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
    string public greeting = "Hello, Blast";

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
export BLAST_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network blast
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
export BLASTSCAN_API_KEY=your_blastscan_api_key
npx hardhat verify --network blast <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

{% hint style="info" %}
On Blast, a contract's ETH and USDB balances default to the `VOID` yield mode (no rebasing). To make a deployed contract's balance accrue yield, configure its yield mode through the Blast yield predeploy at `0x4300000000000000000000000000000000000002` — see the Blast documentation for the `configure` interface.
{% endhint %}
