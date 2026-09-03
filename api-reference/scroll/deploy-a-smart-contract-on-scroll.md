# Deploy a Smart Contract on Scroll

Scroll is EVM-equivalent, so contracts deploy using the standard Solidity toolchain — Foundry, Hardhat, or Remix — with a GetBlock endpoint. This guide covers adding the network to a wallet and deploying a first contract to Scroll.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with ETH on Scroll for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-scroll.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-scroll.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with ETH on Scroll

## Network Details

| Property        | Value                                     |
| --------------- | ----------------------------------------- |
| Network Name    | Scroll Mainnet                            |
| RPC URL         | https://shared.eu-central-1.getblock.io// |
| Chain ID        | 534352 (0x82750)                          |
| Currency Symbol | ETH                                       |
| Block Explorer  | [scrollscan.com](https://scrollscan.com/) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Scroll is not, so it must be added manually — use the button below or the manual steps that follow.

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-scroll.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `534352`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Scroll network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Scroll gas is paid in ETH that has been bridged from Ethereum.

* **Mainnet** — bridge ETH from Ethereum L1 using the official [Scroll Bridge](https://scroll.io/bridge). Bridged ETH becomes spendable on Scroll for gas.
* **Testnet (Scroll Sepolia, chain ID 534351)** — obtain Ethereum Sepolia ETH from a public Sepolia faucet, then bridge it to Scroll Sepolia with the [Scroll Bridge](https://scroll.io/bridge) in testnet mode.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-scroll && cd hello-scroll
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Scroll";

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
export SCROLL_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $SCROLL_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Scroll uses a Scrollscan (Etherscan-compatible) explorer:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 534352 \
  --verifier etherscan \
  --verifier-url https://api.scrollscan.com/api \
  --etherscan-api-key <SCROLLSCAN_API_KEY>
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-scroll && cd hello-scroll
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

const SCROLL_CHAIN_ID = 534352;

module.exports = {
  solidity: '0.8.24',
  networks: {
    scroll: {
      url: process.env.SCROLL_RPC_URL,
      chainId: SCROLL_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { scroll: process.env.SCROLLSCAN_API_KEY },
    customChains: [
      {
        network: 'scroll',
        chainId: SCROLL_CHAIN_ID,
        urls: {
          apiURL: 'https://api.scrollscan.com/api',
          browserURL: 'https://scrollscan.com'
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
    string public greeting = "Hello, Scroll";

    function setGreeting(string calldata greeting_) external {
        greeting = greeting_;
    }
}
```
{% endcode %}
{% endstep %}

{% step %}
### Deploy

{% code overflow="wrap" %}
```bash
export SCROLL_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network scroll
```
{% endcode %}
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
export SCROLLSCAN_API_KEY=your_scrollscan_api_key
npx hardhat verify --network scroll <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}
