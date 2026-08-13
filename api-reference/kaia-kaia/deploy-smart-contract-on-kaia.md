---
description: >-
  This guide covers adding the network to a browser wallet, then deploying a
  simple contract using Foundry and Hardhat — pick whichever you prefer on Kaia.
---

# Deploy smart contract on Kaia

Kaia is EVM-compatible, so contracts deploy with standard Ethereum tooling such as Foundry, Hardhat, and Remix. This guide covers adding Kaia to a wallet and deploying a first contract with Foundry and Hardhat through a GetBlock endpoint. Kaia contracts are verified either through Sourcify or through the KaiaScan verification API.

## Prerequisites

* A wallet holding KAIA for gas (see Add Network to Your Wallet below)
* A GetBlock access token from the GetBlock dashboard, used as `<ACCESS-TOKEN>` in the endpoint `https://go.getblock.io/<ACCESS-TOKEN>/`
* Node.js version 20 or later, for Hardhat
* A funded deployer address, on Kairos Testnet for testing (see Testnet Faucets)

### Network Details

| Property        | Kaia Mainnet                             | Kairos Testnet                                    |
| --------------- | ---------------------------------------- | ------------------------------------------------- |
| Chain ID        | 8217 (0x2019)                            | 1001 (0x3e9)                                      |
| RPC URL         | `https://go.getblock.io/<ACCESS-TOKEN>/` | `https://go.getblock.io/<ACCESS-TOKEN>/`          |
| Currency Symbol | KAIA                                     | KAIA                                              |
| Block Explorer  | [kaiascan.io](https://kaiascan.io/)      | [kairos.kaiascan.io](https://kairos.kaiascan.io/) |

{% hint style="info" %}
Kaia is not pre-configured in MetaMask by default, and is well supported by the Kaia Wallet extension. To use MetaMask, add Kaia manually or through an add-network button before it appears in the wallet.
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

Open the Kaia Wallet extension, or MetaMask.
{% endstep %}

{% step %}
### Open the network dropdown

Click the network dropdown at the top of the wallet.
{% endstep %}

{% step %}
### Add a network manually

Click **Add network**, then **Add a network manually**.
{% endstep %}

{% step %}
### Enter network details

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
mkdir kaia-deploy && cd kaia-deploy
forge init
```
{% endstep %}

{% step %}
### Create a contract

Create `src/HelloKaia.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract HelloKaia {
    function hello() external pure returns (string memory) {
        return "Hello from Kaia";
    }
}
```
{% endstep %}

{% step %}
### Deploy

```bash
export PRIVATE_KEY=<your-deployer-private-key>
export KAIA_RPC_URL=https://go.getblock.io/<ACCESS-TOKEN>/

forge create src/HelloKaia.sol:HelloKaia \
  --rpc-url $KAIA_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify with Sourcify

Foundry has native Sourcify support. Verify the deployed contract against Sourcify:

```bash
forge verify-contract <contract_address> \
  src/HelloKaia.sol:HelloKaia \
  --chain-id 8217 \
  --verifier sourcify \
  --verifier-url https://sourcify.dev/server/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create a project

```bash
mkdir kaia-deploy && cd kaia-deploy
npm init -y
npm install --save-dev hardhat
npx hardhat init
```

Select a JavaScript project when prompted.
{% endstep %}

{% step %}
### Configure networks and verification

KaiaScan exposes an Etherscan-compatible verification endpoint, configured as a custom chain. The API key value is not used and can be any placeholder.

{% code title="hardhat.config.js" %}
```javascript
require('@nomicfoundation/hardhat-toolbox');

const PRIVATE_KEY = process.env.PRIVATE_KEY;

module.exports = {
  solidity: '0.8.24',
  networks: {
    kaia: {
      url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
      chainId: 8217,
      accounts: [PRIVATE_KEY]
    },
    kairos: {
      url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
      chainId: 1001,
      accounts: [PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: {
      kaia: 'unnecessary',
      kairos: 'unnecessary'
    },
    customChains: [
      {
        network: 'kaia',
        chainId: 8217,
        urls: {
          apiURL: 'https://mainnet-api.kaiascan.io/hardhat-verify',
          browserURL: 'https://kaiascan.io'
        }
      },
      {
        network: 'kairos',
        chainId: 1001,
        urls: {
          apiURL: 'https://kairos-api.kaiascan.io/hardhat-verify',
          browserURL: 'https://kairos.kaiascan.io'
        }
      }
    ]
  }
};
```
{% endcode %}
{% endstep %}

{% step %}
### Create a contract

Create `contracts/HelloKaia.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract HelloKaia {
    function hello() external pure returns (string memory) {
        return "Hello from Kaia";
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
  const contract = await hre.ethers.deployContract('HelloKaia');
  await contract.waitForDeployment();
  console.log('HelloKaia deployed to:', await contract.getAddress());
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
npx hardhat run scripts/deploy.js --network kaia
```
{% endstep %}

{% step %}
### Verify on KaiaScan

```bash
npx hardhat verify --network kaia \
  --contract contracts/HelloKaia.sol:HelloKaia <contract_address>
```

On success, the output links to the verified contract on KaiaScan.
{% endstep %}
{% endstepper %}

## Testnet Faucets

### Kairos Testnet

* [Kaia Faucet](https://kaiafaucet.com/) — dispenses Kairos test KAIA to a connected wallet (verify)
