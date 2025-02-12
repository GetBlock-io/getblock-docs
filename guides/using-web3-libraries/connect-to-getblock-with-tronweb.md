---
description: >-
  In this guide, we will show you how to get started with TronWeb to connect to
  GetBlock.
---

# TronWeb integration

TronWeb is a JavaScript library of TRON full node’s API functions that is used to deploy smart contracts, query blockchain and contract information, trade on the decentralized exchanges and change the blockchain state.

Firstly, you will need to add the TronWeb library to your project.

* Npm:

```bash
npm install tronweb
```

* Yarn:&#x20;

```bash
yarn add tronweb
```

In your javascript file, define TronWeb:&#x20;

```javascript
const TronWeb = require('tronweb');
```

When you instantiate TronWeb you can define:

* fullNode
* solidityNode
* eventServer
* privateKey

you can also set a

* fullHost

Which works as a jolly. If you do so, though, the more precise specification has priority. Supposing you are using a server which provides everything, like TronGrid, you can instantiate TronWeb as:

```javascript
const tronWeb = new TronWeb({
fullHost: "https://go.getblock.io/<ACCESS-TOKEN>/"
})
```

For retro-compatibility, though, you can continue to use the old approach, where any parameter is passed separately (using the GetBlock node as an example here):

```javascript
const fullNode = new TronWeb.providers.HttpProvider("https://go.getblock.io/<ACCESS-TOKEN>/")
const solidityNode = new TronWeb.providers.HttpProvider("https://go.getblock.io/<ACCESS-TOKEN>/")
const eventServer = new TronWeb.providers.HttpProvider("https://go.getblock.io/<ACCESS-TOKEN>/")
const tronWeb = new TronWeb(fullNode, solidityNode, eventServer)
```

After this you can call any TronWeb method:

<pre class="language-javascript"><code class="lang-javascript"><strong>tronWeb.trx.getBlock('latest').then(result => {console.log(result)});
</strong></code></pre>

All API references can be found in the project documentation at [https://developers.tron.network/reference](https://developers.tron.network/reference)

## Method response:

![tronWeb.trx.getBlock(blockNumberOrBlockId) method response](https://storage.getblock.io/web/docs/guides/how-to-connect-to-getblock-with-tronweb/tronweb_screenshot_1.webp)
