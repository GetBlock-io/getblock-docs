---
description: >-
  Aptos Network API Reference for seamless interaction with APT nodes, enabling
  fast, secure, and scalable transactions on a next-generation Layer 1
  blockchain.
---

# Aptos (APT)

## Overview of Aptos Network Methods

Aptos is a Layer 1 blockchain built with the [Move programming language](https://move-book.com/), designed to deliver a fast, secure, scalable, and upgradeable ecosystem. With its modular architecture, Aptos enables frequent upgrades, rapid adoption of new technologies, and strong support for emerging use cases.
The Aptos API provides developers with the ability to interact with the blockchain seamlessly and possibly. 
You can do the following with the Aptos API:
  1. Query real-time blockchain data
  2. Manage and monitor accounts and balances
  3. Interact with smart contracts
  4. Submit and track transactions
  5. Monitor network activity in real-time

Each method will provide you with the following:
  1. Clear description of functionality and use cases
  2. Required input parameters
  3. Sample requests and responses
  4. Supported network
  5. Code example
  6. Integration

  > Note: This API is compatible only with Aptos Mainnet.

## Quickstart

In this section, you will learn how to make your first call with either:

* Axios
    
* Python
    

### Quickstart with Axios

Before you begin, you must have already installed `npm` or `yarn` on your local machine. If not, check out [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [yarn](https://classic.yarnpkg.com/lang/en/docs/install).

1. Set up your project using this command:
    
    For npm:
    
    ```bash
    mkdir aptos-api-quickstart
    cd aptos-api-quickstart
    npm init --yes
    ```
    

Or yarn:

```bash
mkdir aptos-api-quickstart
cd aptos-api-quickstart
yarn init --yes
```

This creates a project directory named `aptos-api-quickstart` and initialises a Node.js project within it.

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
    import axios from 'axios'
    
    let account = {
      method: 'get',
      url: 'https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f',
    };
     
    axios.request(account)
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
    
    The sequence number and authentication key log in your console like this:
    
    ```json
    {
      "Sequence_number": "219241",
      "Authentication_key": "0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f"
    }
    ```
    

### Quickstart with Python and Requests

Before you begin, you must have installed Python and Pip on your local machine.

1. Set up your project using this command:
    
    ```bash
    mkdir aptos-api-quickstart
    cd aptos-api-quickstart
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
    
    url = "https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f"
    
    response = requests.get(url)
    
    print(response.text)
    ```
    
    > Replace `<ACCESS_TOKEN>` with your actual access token from GetBlock.
    
5. Run the script:
    
    ```bash
    python main.py
    ```

### Endpoint Grouping

1. Blockchain Information
    
    * /v1
        
2. Account-Related
    
    * /v1/accounts/{account\_hash}
        
    * /v1/accounts/{account\_hash}/resources
        
    * /v1/accounts/{account\_hash}/resource/{resource\_type}
        
    * /v1/accounts/{account\_hash}/modules
        
    * /v1/accounts/{account\_hash}/module/{module\_name}
        
    * /v1/accounts/{account\_hash}/transactions
        
3. Events
    
    * /v1/accounts/{account\_hash}/events/{creation\_number}
        
    * /v1/accounts/{account\_hash}/events/{event\_handle}/{field\_name}
        
4. Blocks
    
    * /v1/blocks/by\_height/{block\_height}
        
    * /v1/blocks/by\_version/{version}
        
5. Transactions
    
    * /v1/transactions
        
    * /v1/transactions/by\_hash/{transaction\_hash}
        
    * /v1/transactions/by\_version/{version}
        
6. Additional Utility
    
    * /v1/estimate\_gas\_price
