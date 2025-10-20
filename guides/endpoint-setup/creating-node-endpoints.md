---
description: >-
  Follow the steps below to set up an endpoint and generate access tokens for
  your project.
---

# Creating node endpoints

This short guide shows you how to **create an RPC endpoint** (an RPC URL) for any supported protocol in your GetBlock Shared Node dashboard to connect it to your app, script, or wallet.&#x20;

{% hint style="success" %}
In GetBlock, an _endpoint URL_ includes your unique **Access Token** — the credential that authenticates RPC requests. GetBlock’s UI sometimes labels the whole endpoint provisioning flow “Get Access Token” because a new RPC URL is created together with the token.&#x20;

Related:

* [Access token management ](../../getting-started/authentication-with-access-tokens.md)
* [GetBlock Deploys Major Security Upgrade: Introducing Access Tokens](https://getblock.io/blog/getblock-deploys-major-security-upgrade-introducing-access-tokens/)
{% endhint %}

***

The steps below cover how to generate a new endpoint URL with an Access Token:

{% stepper %}
{% step %}
Log in to your GetBlock account and navigate to the **Dashboard**
{% endstep %}

{% step %}
Find the **Endpoints** section on the Dashboard
{% endstep %}

{% step %}
Click **Get endpoint** to open the endpoint setup menu

<figure><img src="../../.gitbook/assets/Endpoint_setup_Oct &#x27;25.png" alt="GetBlock RPC endpoint setup interface"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
In the modal that opens, select:

* The desired blockchain **protocol** (Ethereum, BNB Chain, Polygon, etc.)
* The **network** you want to interact with: mainnet or testnet
* Node **mode:** full (default) or archive
* The **API** interface that you need (JSON-RPC, WebSockets, GraphQL, etc.)
* One of the available server **locations** (Frankfurt, New York, or Singapore)&#x20;

<figure><img src="../../.gitbook/assets/Create_access_token_modal.svg" alt="How to create a node endpoint for blockchain API access"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
Click 'Get' and have the endpoint URL with an access token generated.
{% endstep %}
{% endstepper %}

Generate and add as many access tokens as required for this protocol. Each token is a unique endpoint for you and your application to interact with the blockchain.

{% hint style="info" %}
All GetBlock endpoints follow a predictable format. The visible difference is the hostname reflecting the region selected during the setup.&#x20;

**Endpoint examples**:

```markup
EU (Frankfurt):   https://go.getblock.io/<ACCESS_TOKEN>/
US (New York):    https://go.getblock.us/<ACCESS_TOKEN>/
Asia (Singapore): https://go.getblock.asia/<ACCESS_TOKEN>/
```

The token encodes the protocol, networks, and routing on the server — clients don’t need to specify a chain in the URL.
{% endhint %}

***

### Full vs Archive mode

When creating an endpoint in your GetBlock Dashboard, for select protocols, you can choose between two node access modes – Full and Archive. This selection determines how much historical blockchain data your endpoint can access.\


* **Full mode**: Standard full (pruned) node behavior — current state lookups, sending transactions, reading blocks, etc.
* **Archive mode**: Enables access to the historical chain state. Useful for querying balances, contract storage, UTXO sets, executing historical calls, simulating transactions at a past block, or reconstructing chain state for analytics and audits.&#x20;

{% hint style="info" %}
Selecting the Archive mode for an endpoint changes how requests are billed in Compute Units (CU). Learn more in the [Archive mode guide](enabling-archive-mode.md).
{% endhint %}

***

### Viewing and managing endpoints

The created URL is shown on the endpoints list so you can copy it and start calling the node. Use the right-side menu (![](../../.gitbook/assets/dots-horizontal.svg)) to roll (regenerate) or delete the endpoint from the list. &#x20;

<figure><img src="../../.gitbook/assets/Endpoints_list (Oct &#x27;25).svg" alt="Blockchain RPC nodes list within the GetBlock account"><figcaption></figcaption></figure>

{% hint style="warning" %}
Because the Access Token is embedded, **the URL is the credential**. Keep it secret and store securely. If the URL is exposed, regenerate or revoke it from your GetBlock account.
{% endhint %}
