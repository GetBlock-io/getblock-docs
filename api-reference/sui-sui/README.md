---
description: >-
  GetBlock provides fast and reliable access to Sui RPC nodes via JSON-RPC API.
  Connect to the SUI network without running your own infrastructure.
---

# Sui (SUI)

SUI is a high-performance Layer 1 blockchain designed for low-latency, high-throughput applications. Built by Mysten Labs using the Move programming language, SUI features a unique object-centric data model that enables parallel transaction execution. The network achieves sub-second finality through its innovative Narwhal and Bullshark (transitioning to Mysticeti) consensus mechanism, making it ideal for gaming, DeFi, and real-time applications.

{% hint style="warning" %}
**JSON RPC is deprecated. Migrate to gRPC, as JSON-RPC will be fully deactivated by July 2026.** \
\
**Why gPRC?**

gRPC (Google Remote Procedure Call) is a modern, high-performance protocol that uses [Protocol Buffers ](https://protobuf.dev/overview/)for fast, compact data serialization. It offers strongly typed interfaces, built-in streaming support, and significantly better performance than JSON-RPC — making it ideal for indexers, trading bots, and data-intensive applications.
{% endhint %}

### Key Features

* **Object-Centric Model**: Unique architecture where everything is an object with ownership properties
* **Move Programming Language**: Rust-based, asset-oriented smart contract language with built-in safety
* **Parallel Transaction Execution**: Independent transactions are processed simultaneously for high throughput
* **Sub-Second Finality**: \~400ms average transaction finality with Mysticeti consensus
* **Horizontal Scalability**: Designed for 100,000+ TPS through parallel processing
* **SUI Native Token**: Uses SUI for gas fees, staking, and governance (9 decimals, smallest unit: MIST)
* **Dynamic NFTs**: Native support for evolving, composable digital assets
* **Sponsored Transactions**: Gas payment abstraction for better user experience
* **SuiNS**: Native name service for human-readable addresses

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to streamline the developer experience. The canonical and normative specification for SUI gRPC methods is solely maintained and published through the official SUI documentation portal at_ [_docs.sui.io_](https://docs.sui.io/)_. This resource constitutes the sole authoritative reference implementation of the gRPC protocol interface for SUI network clients._
{% endhint %}

## Network Information

| Property        | Value                          |
| --------------- | ------------------------------ |
| Network Name    | SUI Mainnet                    |
| Chain ID        | 101                            |
| Native Currency | SUI                            |
| Decimals        | 9 (1 SUI = 1,000,000,000 MIST) |
| gRPC Port       | 443 (TLS)                      |
| Protocol        | gRPC with Protocol Buffers     |

## Base URL

| Location           | gRPC Endpoint             |
| ------------------ | ------------------------- |
| Frankfurt, Germany | `https://go.getblock.io/` |

## Supported Networks

| Network | Chain ID | gRPC |
| ------- | -------: | ---- |
| Mainnet |      101 | ✅    |

## Quickstart

In this section, you will learn how to make your first gRPC call using TypeScript/Node.js.

{% hint style="warning" %}
You must have `npm` or `yarn` installed on your local machine.

* [npm installation guide](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
* [yarn installation guide](https://classic.yarnpkg.com/lang/en/docs/install)
{% endhint %}

### Quickstart with TypeScript/Node.js

{% stepper %}
{% step %}
### Set up the project

Create and initialize a new project:

```bash
mkdir sui-grpc-quickstart
cd sui-grpc-quickstart
npm init --yes
```
{% endstep %}

{% step %}
### Install dependencies

```bash
npm install @grpc/grpc-js @grpc/proto-loader
npm install -D typescript ts-node @types/node
```
{% endstep %}

{% step %}
### Initialize TypeScript

```bash
npx tsc --init
```
{% endstep %}

{% step %}
### Download proto files

Clone the official SUI proto files:

```bash
git clone https://github.com/MystenLabs/sui-apis.git protos
```

Your folder structure should look like:

```
sui-grpc-quickstart/
├── protos/
│   └── proto/
│       ├── google/
│       │   ├── protobuf/
│       │   └── rpc/
│       └── sui/
│           └── rpc/
│               └── v2/
│                   ├── ledger_service.proto
│                   ├── state_service.proto
│                   └── ...
├── index.ts
├── package.json
└── tsconfig.json
```
{% endstep %}

{% step %}
### Create the client file

Create a new file named `index.ts`:

{% code overflow="wrap" %}
```typescript
import * as grpc from '@grpc/grpc-js';
import * as protoLoader from '@grpc/proto-loader';
import * as path from 'path';

// Path to the proto file
const PROTO_PATH = path.join(__dirname, 'protos/proto/sui/rpc/v2/ledger_service.proto');

// Load proto definitions
const packageDefinition = protoLoader.loadSync(PROTO_PATH, {
    keepCase: true,
    longs: String,
    enums: String,
    defaults: true,
    oneofs: true,
    includeDirs: [path.join(__dirname, 'protos/proto')],
});

const suiProto = grpc.loadPackageDefinition(packageDefinition) as any;
const LedgerService = suiProto.sui.rpc.v2.LedgerService;

// GetBlock gRPC endpoint — access token is part of the URL path
const ACCESS_TOKEN = "<ACCESS_TOKEN>";
const GETBLOCK_GRPC_URL = `go.getblock.io:443`;

// Create metadata with access token
const metadata = new grpc.Metadata();
metadata.add('x-api-key', ACCESS_TOKEN);

// Create gRPC client with TLS
const client = new LedgerService(
    GETBLOCK_GRPC_URL,
    grpc.credentials.createSsl()
);

// Example 1: Get Service Info
function getServiceInfo(): Promise<any> {
    return new Promise((resolve, reject) => {
        client.GetServiceInfo({}, metadata, (err: any, response: any) => {
            if (err) reject(err);
            else resolve(response);
        });
    });
}

// Example 2: Get Checkpoint
function getCheckpoint(sequenceNumber: string): Promise<any> {
    return new Promise((resolve, reject) => {
        const request = {
            sequence_number: sequenceNumber,
            read_mask: {
                paths: ['sequence_number', 'digest', 'summary']
            }
        };
        client.GetCheckpoint(request, metadata, (err: any, response: any) => {
            if (err) reject(err);
            else resolve(response);
        });
    });
}

// Example 3: Get Transaction
function getTransaction(digest: string): Promise<any> {
    return new Promise((resolve, reject) => {
        const request = {
            digest: digest,
            read_mask: {
                paths: ['digest', 'effects', 'events']
            }
        };
        client.GetTransaction(request, metadata, (err: any, response: any) => {
            if (err) reject(err);
            else resolve(response);
        });
    });
}

// Main function
async function main() {
    try {
        // Get service info
        console.log('📡 Fetching service info...\n');
        const serviceInfo = await getServiceInfo();
        console.log('Service Info:', JSON.stringify(serviceInfo, null, 2));

        // Get latest checkpoint
        console.log('\n📦 Fetching checkpoint #1000...\n');
        const checkpoint = await getCheckpoint('1000');
        console.log('Checkpoint:', JSON.stringify(checkpoint, null, 2));

    } catch (error) {
        console.error('Error:', error);
    }
}

main();
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual GetBlock access token.
{% endstep %}

{% step %}
### Update `tsconfig.json`

Ensure your `tsconfig.json` has these settings:

{% code overflow="wrap" %}
```json
{
  "compilerOptions": {
     "target": "ES2020",
    "module": "commonjs",
    "esModuleInterop": true,
    "outDir": "./dist",
    "rootDir": "./",
    "types": [],
    "sourceMap": true,
    "declaration": true,
    "declarationMap": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    "strict": true,
    "jsx": "react-jsx",
    "isolatedModules": true,
    "noUncheckedSideEffectImports": true,
    "moduleDetection": "force",
    "skipLibCheck": true,
  }
}
```
{% endcode %}
{% endstep %}

{% step %}
### Run the script

```bash
npx ts-node index.ts
```

**Expected output:**

```json
📡 Fetching service info...

Service Info: {
  "chain_id": "4btiuiMPvEENsttpZC7CZ53DruC3MAgfznDbASZ7DR6S",
  "chain": "mainnet",
  "epoch": "758",
  "checkpoint_height": "231612345",
  "lowest_available_checkpoint": "0",
  "server": "sui-node/1.45.0"
}

📦 Fetching checkpoint #1000...

Checkpoint: {
  "checkpoint": {
    "sequence_number": "1000",
    "digest": "8Jq2xK...",
    "summary": { ... }
  }
}
```
{% endstep %}
{% endstepper %}

## More Examples

### Get Balance (StateService)

To query balances, use the `StateService`:

```typescript
import * as grpc from '@grpc/grpc-js';
import * as protoLoader from '@grpc/proto-loader';
import * as path from 'path';

const PROTO_PATH = path.join(__dirname, 'protos/proto/sui/rpc/v2/state_service.proto');

const packageDefinition = protoLoader.loadSync(PROTO_PATH, {
    keepCase: true,
    longs: String,
    enums: String,
    defaults: true,
    oneofs: true,
    includeDirs: [path.join(__dirname, 'protos/proto')],
});

const suiProto = grpc.loadPackageDefinition(packageDefinition) as any;
const StateService = suiProto.sui.rpc.v2.StateService;

const GETBLOCK_GRPC_URL = 'grpc.getblock.io:443';
const API_KEY = '<ACCESS-TOKEN>';

const metadata = new grpc.Metadata();
metadata.add('x-api-key', API_KEY);

const client = new StateService(
    GETBLOCK_GRPC_URL,
    grpc.credentials.createSsl()
);

// Get SUI balance for an address
function getBalance(owner: string, coinType: string): Promise<any> {
    return new Promise((resolve, reject) => {
        const request = {
            owner: owner,
            coin_type: coinType
        };
        client.GetBalance(request, metadata, (err: any, response: any) => {
            if (err) reject(err);
            else resolve(response);
        });
    });
}

async function main() {
    const balance = await getBalance(
        '0x94096a6a54129234237759c66e6ef1037224fb3102a0ae29d33b490281c8e4d5',
        '0x2::sui::SUI'
    );
    console.log('Balance:', JSON.stringify(balance, null, 2));
}

main();
```

### List Owned Objects

```typescript
function listOwnedObjects(owner: string, pageSize: number = 10): Promise<any> {
    return new Promise((resolve, reject) => {
        const request = {
            owner: owner,
            page_size: pageSize,
            read_mask: {
                paths: ['object_id', 'version', 'object_type']
            }
        };
        client.ListOwnedObjects(request, metadata, (err: any, response: any) => {
            if (err) reject(err);
            else resolve(response);
        });
    });
}
```

### Get Coin Info

```typescript
function getCoinInfo(coinType: string): Promise<any> {
    return new Promise((resolve, reject) => {
        const request = {
            coin_type: coinType
        };
        client.GetCoinInfo(request, metadata, (err: any, response: any) => {
            if (err) reject(err);
            else resolve(response);
        });
    });
}

// Usage
const coinInfo = await getCoinInfo('0x2::sui::SUI');
console.log('Coin Info:', coinInfo);
```

## Available gRPC Services & Methods

### LedgerService

| Method                 | Description                                         |
| ---------------------- | --------------------------------------------------- |
| `GetServiceInfo`       | Returns node info (chain, epoch, checkpoint height) |
| `GetObject`            | Returns object data by ID                           |
| `BatchGetObjects`      | Returns multiple objects by IDs                     |
| `GetTransaction`       | Returns transaction by digest                       |
| `BatchGetTransactions` | Returns multiple transactions                       |
| `GetCheckpoint`        | Returns checkpoint by sequence number or digest     |
| `GetEpoch`             | Returns epoch information                           |

### StateService

| Method              | Description                                      |
| ------------------- | ------------------------------------------------ |
| `GetCoinInfo`       | Returns coin metadata (symbol, decimals, supply) |
| `GetBalance`        | Returns balance for address and coin type        |
| `ListBalances`      | Returns all balances for an address              |
| `ListOwnedObjects`  | Returns objects owned by an address              |
| `ListDynamicFields` | Returns dynamic fields of an object              |

### TransactionExecutionService

| Method                | Description                               |
| --------------------- | ----------------------------------------- |
| `ExecuteTransaction`  | Submits and executes a signed transaction |
| `SimulateTransaction` | Dry-runs a transaction without execution  |

### SubscriptionService

| Method                 | Description                     |
| ---------------------- | ------------------------------- |
| `SubscribeCheckpoints` | Streams live checkpoint updates |

### MovePackageService

| Method        | Description                      |
| ------------- | -------------------------------- |
| `GetPackage`  | Returns Move package metadata    |
| `GetFunction` | Returns Move function definition |

### NameService

| Method          | Description                          |
| --------------- | ------------------------------------ |
| `ResolveName`   | Resolves SuiNS name to address       |
| `LookupAddress` | Reverse lookup address to SuiNS name |

## Field Masks

Use `read_mask` to request only the fields you need:

```typescript
const request = {
    sequence_number: '1000',
    read_mask: {
        paths: ['sequence_number', 'digest', 'summary', 'transactions']
    }
};
```

Use `*` to get all fields:

```typescript
const request = {
    sequence_number: '1000',
    read_mask: {
        paths: ['*']
    }
};
```

## Error Handling

```typescript
try {
    const result = await getCheckpoint('1000');
    console.log(result);
} catch (error: any) {
    if (error.code === grpc.status.NOT_FOUND) {
        console.error('Checkpoint not found');
    } else if (error.code === grpc.status.UNAUTHENTICATED) {
        console.error('Invalid API key');
    } else {
        console.error('gRPC Error:', error.message);
    }
}
```

## Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Sui gRPC Documentation](https://docs.sui.io/concepts/data-access/grpc-overview)
* [Official Proto Files (sui-apis)](https://github.com/MystenLabs/sui-apis)
* [Sui Developer Documentation](https://docs.sui.io/)
* [Sui TypeScript SDK](https://sdk.mystenlabs.com/typescript)
* [Protocol Buffers Documentation](https://protobuf.dev/)
* [Sui Bridge](https://bridge.sui.io/)
* [Sui Rust SDK](https://docs.sui.io/references/rust-sdk)
* [Move Language Documentation](https://move-language.github.io/move/)
