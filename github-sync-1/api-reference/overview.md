---
description: >-
  GetBlock API docs: RPC endpoints, SDK guides, and developer tools. Everything
  you need to connect and build on 100+ blockchain networks
icon: hammer
---

# Overview

## Getting Started with GetBlock

To get started, follow these steps:

1. Sign up: Create an account to access your Access Token.
2. Generate an access token: Navigate to the dashboard and generate your first access token for API authentication.
3. Choose the blockchain name and type: Select the blockchain network you want to interact with, set up testnet/mainnet, and choose the interface you’re going to use.
4. Send your first request:

{% code overflow="wrap" %}
```python
import requests
url = "https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/"
headers = { "Content-Type": "application/json" }
payload = { "jsonrpc": "2.0", "method": "eth_blockNumber", "params": [], "id": "getblock.io" }
response = requests.post(url, headers=headers, json=payload)
print(response.json())
```
{% endcode %}

### Key Features of GetBlock

GetBlock offers one of the most comprehensive API toolkits in the segment, supporting hundreds of dApps with fast, reliable connections to blockchain nodes.

#### 130+ blockchains in store

* Seamless connection to full and archive nodes
* Shared and dedicated nodes: Tailored for your dApp’s needs.
* Programmable and non-programmable blockchains
* L1 and L2 protocols
* EVM and non-EVM networks

See full list: [https://getblock.io/nodes/](https://getblock.io/nodes/)\
\
All mainstream RPC interfaces

* WebSockets
* JSON RPC
* GraphQL
* REST API
* And more

Add them in Lego-like manner: [https://account.getblock.io/](https://account.getblock.io/)

#### Industry-leading suite of add-ons and ready-made APIs

* DAS API
* Firehose
* Blockbook
* Yellowstone Geyser

Need more? Don’t hesitate to contact sales: [https://getblock.io/contact/](https://getblock.io/contact/)

## Examples for Console REST API Requests

#### cURL

Most \*nix-based systems come with cURL pre-installed. cURL is a command-line tool and library for transferring data with URLs. To check if cURL is installed, run the following command:

```json
curl -h
```

Example of requesting the latest block number using the GetBlock API and cURL:

```json
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
    "id": "getblock.io"
}'
```

#### Python

To run Python examples, ensure Python is installed along with the requests package. You can install the package using the following command:

```json
python -m pip install requests
```

Example:

{% code overflow="wrap" %}
```python
import requests
url = "https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/"
headers = { "Content-Type": "application/json" }
payload = { "jsonrpc": "2.0", "method": "eth_blockNumber", "params": [], "id": "getblock.io" }
response = requests.post(url, headers=headers, json=payload)
print(response.json())
```
{% endcode %}

#### JavaScript

For JavaScript examples, you'll need Node.js version 18 or later. Follow the official documentation to install the latest stable version globally. Verify your installation by running:

```
node -v
```

#### Ruby

To execute Ruby examples, install Ruby on your machine. Refer to the official installation guide for details. Confirm installation by running:

```
ruby -v
```
