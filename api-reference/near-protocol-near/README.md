---
description: >-
  NEAR Protocol API Reference for seamless interaction with NEAR nodes, enabling
  scalable, developer-friendly, decentralised applications with low fees and fast
  transaction finality.
---

# Overview of NEAR Protocol (NEAR)
NEAR Protocol is a developer-friendly Layer 1 blockchain built to make decentralised applications (dApps) fast, user-friendly, and scalable.

It uses a Proof of Stake (PoS) consensus mechanism to validate transactions while sharding the network into smaller pieces that work in parallel. This design allows NEAR to process transactions quickly and efficiently with low fees.

Smart contracts on NEAR can be written in:

- [JavaScript](https://www.learn-js.org/)
- [Rust](https://www.rust-lang.org/)
- [Python](https://www.python.org/)
- [Go](https://go.dev/)

Beyond just being a blockchain, NEAR is also a platform for AI agents. Its vision is to create a future of User-Owned AI, where artificial intelligence and autonomous agents serve their users — not corporations.

You can do the following with NEAR Protocol API:
  1. Get block, transaction and network information
  2.  Get account information
  3. Send and validate transactions 
  4. Get estimate of gas and fees
  5. Interact with smart contracts
  6. Monitor network health

## NEAR Network Support
  | Network | API Interface |
  |---------|---------------|
  | Mainnet | JSON RPC      |

Each method will provide you with the following:

  - Clear description of functionality and use cases
  
  - Required input parameters
  
  - Sample requests and responses
  
  - Supported network
  
  - Code example
  
  - Integration

## Quickstart

In this section, you will learn how to make your first call with either:

  * Axios
      
  * Python
    

### Quickstart with Axios

Before you begin, you must have already installed `npm` or `yarn` on your local machine. If not, check out [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [yarn](https://classic.yarnpkg.com/lang/en/docs/install).

1. Set up your project using this command:
    
    For npm:
    
    ```bash
    mkdir near-api-quickstart
    cd near-api-quickstart
    npm init --yes
    ```
    
    Or yarn:
    
    ```bash
    mkdir near-api-quickstart
    cd near-api-quickstart
    yarn init --yes
    ```

This creates a project directory named `near-api-quickstart` and initialises a Node.js project within it.

2. Install Axios using this command:  
    Using npm:
    
    ```bash
    npm install axios
    ```
    
    Using yarn:
    
    ```bash
    yarn add axios
    ```
    
3. Create a new file and name it `index.js`. This is where you will make your first call.
    
4. Set ES module `"type": "module"` in your `package.json`.
    
5. Add the following code to the file (`index.js`):
    
    ```js
    import axios from "axios"
    let data = JSON.stringify({
      "jsonrpc": "2.0",
      "method": "gas_price",
      "params": [
        169528908
      ],
      "id": "getblock.io"
    });
    
    let config = {
      method: 'post',
      maxBodyLength: Infinity,
      url: 'https://go.getblock.io/<ACCESS_TOKEN>',
      headers: { 
        'Content-Type': 'application/json'
      },
      data : data
    };
    
    axios.request(config)
    .then((response) => {
      console.log(JSON.stringify(response.data));
    })
    .catch((error) => {
      console.log(error);
    });

    ```
    
    > Replace `<ACCESS_TOKEN>` with your actual access token from GetBlock.
    
6. Run the script:
    
    ```bash
    node index.js
    ```
    
    The current gas price will log in the console like this:
    
    ```json
     {
        "jsonrpc": "2.0",
        "result": {
            "gas_price": "100000000"
        },
        "id": "getblock.io"
    }
    ```
    

### Quickstart with Python and Requests

Before you begin, you must have installed Python and Pip on your local machine.

1. Set up your project using this command:
    
    ```bash
    mkdir near-api-quickstart
    cd near-api-quickstart
    ```
    
2. Set up a virtual environment to isolate dependencies:
    
    ```bash
    python -m venv venv
    source venv/bin/activate  
    # On Windows, use venv\Scripts\activate
    ```
    
3. Install the requests library:
    
    ```bash
    pip install requests
    ```
    
4. Create a new file called [`main.py`](http://main.py) and insert the following code:
    
    ```python
     import requests
    import json
    
    url = "https://go.getblock.io/<ACCESS_TOKEN>"
    
    payload = json.dumps({
      "jsonrpc": "2.0",
      "method": "gas_price",
      "params": [
        169528908
      ],
      "id": "getblock.io"
    })
    headers = {
      'Content-Type': 'application/json'
    }
    
    response = requests.request("POST", url, headers=headers, data=payload)
    
    print(response.text)

    ```
    
    > Replace `<ACCESS_TOKEN>` with your actual access token from GetBlock.
    
5. Run the script:
    
    ```bash
    python main.py
    ```

## Method Grouping
1. Network Information
   - network_info
   - validators
   - status
2. Blocks and Transactions
    - block 
    - chunk
    - tx
    - broadcast_tx_async
    - broadcast_tx_commit
3. Contracts
    - call_function
    - query(view_code)
    - query(view_state)
4. Gas and Fees
    - gas_price
5. Account
    - query(view_account)
    - query(view_account_basic)
