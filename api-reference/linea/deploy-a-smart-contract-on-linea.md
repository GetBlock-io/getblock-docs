---
description: >-
  This guide covers adding the network to a browser wallet, then deploying a
  simple contract using Foundry and Hardhat — pick whichever you prefer on
  Linea.
---

# How to Deploy A Smart Contract on Linea

Linea is an EVM-equivalent zkEVM that runs standard EVM bytecode, so contracts deploy with the usual Ethereum tooling such as Foundry, Hardhat, and Remix without modification. This guide covers adding Linea to a wallet and deploying a first contract with Foundry and Hardhat through a GetBlock endpoint.

## Prerequisites

* A wallet holding ETH on Linea for gas (see Add Network to Your Wallet below)
* A GetBlock access token from the GetBlock dashboard, used as `<ACCESS-TOKEN>` in the endpoint `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* Node.js version 20 or later, for Hardhat
* A funded deployer address, on Linea Sepolia for testing (see Testnet Faucets)

### Network Details

| Property        | Linea Mainnet                               | Linea Sepolia Testnet                                       |
| --------------- | ------------------------------------------- | ----------------------------------------------------------- |
| Chain ID        | 59144 (0xe708)                              | 59141 (0xe705)                                              |
| RPC URL         | `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`    | `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`                    |
| Currency Symbol | ETH                                         | ETH                                                         |
| Block Explorer  | [lineascan.build](https://lineascan.build/) | [sepolia.lineascan.build](https://sepolia.lineascan.build/) |

{% hint style="info" %}
Linea is not pre-configured in MetaMask by default. Only Ethereum Mainnet and Sepolia ship pre-configured, so Linea must be added manually or through an add-network button before it appears in the wallet.
{% endhint %}

{% hint style="warning" %}
Every command below defaults to Mainnet — replace environment variable values with testnet ones for a dry run before touching real funds.
{% endhint %}

{% hint style="warning" %}
Never commit a real private key. Use environment variables and prefer a throwaway deployer key for testing. Private keys committed to a public repo are drained within seconds by automated bots.
{% endhint %}

## Add Network to Your Wallet

{% stepper %}
{% step %}
### Open your wallet

Open MetaMask, or the EVM wallet of choice.
{% endstep %}

{% step %}
### Open the network menu

Click the network dropdown at the top of the wallet.
{% endstep %}

{% step %}
### Add a network manually

Click **Add network**, then **Add a network manually**.
{% endstep %}

{% step %}
### Enter the network details

Enter the network details from the Prerequisites section above.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added network.
{% endstep %}
{% endstepper %}

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
```
{% endstep %}

{% step %}
### Create a project

```bash
mkdir linea-deploy && cd linea-deploy
forge init
```
{% endstep %}

{% step %}
### Create a contract

Create `src/HelloLinea.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.30;

contract HelloLinea {
    function hello() external pure returns (string memory) {
        return "Hello from Linea";
    }
}
```
{% endstep %}

{% step %}
### Deploy

```bash
export PRIVATE_KEY=<your-deployer-private-key>
export LINEA_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

forge create src/HelloLinea.sol:HelloLinea \
  --rpc-url $LINEA_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on Block Explorer

LineaScan uses the Etherscan V2 unified API. Use the chain ID and a single Etherscan API key:

```bash
export ETHERSCAN_API_KEY=<your-etherscan-api-key>

forge verify-contract <contract_address> \
  src/HelloLinea.sol:HelloLinea \
  --chain-id 59144 \
  --etherscan-api-key $ETHERSCAN_API_KEY
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create a project

```bash
mkdir linea-deploy && cd linea-deploy
npm init -y
npm install --save-dev hardhat
npx hardhat init
```

Select a JavaScript project when prompted.
{% endstep %}

{% step %}
### Configure networks

{% code title="hardhat.config.js" %}
```javascript
require('@nomicfoundation/hardhat-toolbox');

const PRIVATE_KEY = process.env.PRIVATE_KEY;

module.exports = {
  solidity: '0.8.30',
  networks: {
    linea: {
      url: 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
      chainId: 59144,
      accounts: [PRIVATE_KEY]
    },
    lineaSepolia: {
      url: 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
      chainId: 59141,
      accounts: [PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: process.env.ETHERSCAN_API_KEY
  }
};
```
{% endcode %}
{% endstep %}

{% step %}
### Create a contract

Create `contracts/HelloLinea.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.30;

contract HelloLinea {
    function hello() external pure returns (string memory) {
        return "Hello from Linea";
    }
}
```
{% endstep %}

{% step %}
### Compile and Deploy

Create `scripts/deploy.js`:

{% code title="scripts/deploy.js" %}
```javascript
const hre = require('hardhat');

async function main() {
  const contract = await hre.ethers.deployContract('HelloLinea');
  await contract.waitForDeployment();
  console.log('HelloLinea deployed to:', await contract.getAddress());
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```
{% endcode %}

Then compile and deploy:

```bash
npx hardhat compile
npx hardhat run scripts/deploy.js --network linea
```
{% endstep %}

{% step %}
### Verify on Block Explorer

```bash
npx hardhat verify --network linea <contract_address>
```
{% endstep %}
{% endstepper %}

## Testnet Faucets

### Linea Sepolia

* [Linea Faucet](https://faucet.linea.build/) — dispenses Linea Sepolia ETH to a connected wallet (verify)
* Bridge Sepolia ETH from Ethereum Sepolia through the [Linea Bridge](https://bridge.linea.build/) as an alternative to faucets
