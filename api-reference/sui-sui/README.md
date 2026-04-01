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
npm install @grpc/proto-loader
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

{% code title="index.ts" overflow="wrap" %}
```typescript
import * as https from 'https';
import * as path from 'path';
import * as protobuf from 'protobufjs';

const ACCESS_TOKEN = "<ACCESS_TOKEN>";
const HOST = "go.getblock.io";
const PROTO_ROOT = path.join(__dirname, 'protos/proto');

// Encode request + wrap in gRPC-Web 5-byte frame, then POST over HTTPS
function grpcWebCall(
    root: protobuf.Root,
    servicePath: string,
    method: string,
    requestType: string,
    responseType: string,
    requestData: object
): Promise<object> {
    const ReqType = root.lookupType(requestType);
    const ResType = root.lookupType(responseType);

    const encoded = ReqType.encode(ReqType.create(requestData)).finish();

    // gRPC-Web frame: 1-byte flags (0 = no compression) + 4-byte big-endian length + body
    const frame = Buffer.alloc(5 + encoded.length);
    frame.writeUInt8(0, 0);
    frame.writeUInt32BE(encoded.length, 1);
    Buffer.from(encoded).copy(frame, 5);

    return new Promise((resolve, reject) => {
        const req = https.request(
            {
                hostname: HOST,
                port: 443,
                path: `/${ACCESS_TOKEN}/${servicePath}/${method}`,
                method: 'POST',
                headers: {
                    'content-type': 'application/grpc-web+proto',
                    'x-grpc-web': '1',
                    'accept': 'application/grpc-web+proto',
                    'content-length': frame.length,
                },
            },
            (res) => {
                console.log('HTTP status  :', res.statusCode);
                console.log('Content-Type :', res.headers['content-type']);

                const chunks: Buffer[] = [];
                res.on('data', (chunk: Buffer) => chunks.push(chunk));
                res.on('end', () => {
                    const data = Buffer.concat(chunks);
                    if (data.length < 5) {
                        return reject(new Error(`Short response: ${data.toString('hex')}`));
                    }

                    const flags = data.readUInt8(0);
                    const length = data.readUInt32BE(1);

                    // Trailer frame (flags bit 7 set) contains gRPC status metadata
                    if (flags & 0x80) {
                        const trailer = data.slice(5).toString();
                        return reject(new Error(`gRPC-Web error trailer: ${trailer}`));
                    }

                    const responseBytes = data.slice(5, 5 + length);
                    const response = ResType.decode(responseBytes);
                    resolve(ResType.toObject(response, { longs: String, enums: String, defaults: true }));
                });
            }
        );

        req.on('error', reject);
        req.write(frame);
        req.end();
    });
}

async function main() {
    const root = new protobuf.Root();
    root.resolvePath = (_origin: string, target: string) =>
        target.startsWith('/') ? target : path.join(PROTO_ROOT, target);
    await root.load(path.join(PROTO_ROOT, 'sui/rpc/v2/ledger_service.proto'), { keepCase: true });

    console.log('Fetching service info...\n');
    const info = await grpcWebCall(
        root,
        'sui.rpc.v2.LedgerService',
        'GetServiceInfo',
        'sui.rpc.v2.GetServiceInfoRequest',
        'sui.rpc.v2.GetServiceInfoResponse',
        {}
    );
    console.log('Service Info:', JSON.stringify(info, null, 2));
}

main().catch(console.error);

```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual GetBlock access token.
{% endstep %}

{% step %}
### Update `tsconfig.json`

Ensure your `tsconfig.json` has these settings:

{% code title="tsconfig.json" overflow="wrap" %}
```json
{
  "compilerOptions": {
     "target": "ES2020",
    "module": "commonjs",
    "esModuleInterop": true,
    "outDir": "./dist",
    "rootDir": "./",
    "types": ["node"],
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
Fetching service info...

HTTP status  : 200
Content-Type : application/grpc-web+proto
Service Info: {
  "chain_id": "4btiuiMPvEENsttpZC7CZ53DruC3MAgfznDbASZ7DR6S",
  "chain": "mainnet",
  "epoch": "1084",
  "checkpoint_height": "260411497",
  "timestamp": {
    "seconds": "1775050511",
    "nanos": 19000000
  },
  "lowest_available_checkpoint": "259896745",
  "lowest_available_checkpoint_objects": "259896745",
  "server": "sui-node/1.68.1-3c0f387ebb40"
}
```
{% endstep %}
{% endstepper %}

## More Examples

### Get Balance (StateService)

To query balances, use the `StateService`:

{% code overflow="wrap" %}
```typescript
import * as https from 'https';
import * as path from 'path';
import * as protobuf from 'protobufjs';

const ACCESS_TOKEN = "<ACCESS_TOKEN>";
const HOST = "go.getblock.io";
const PROTO_ROOT = path.join(__dirname, 'protos/proto');

// Encode request + wrap in gRPC-Web 5-byte frame, then POST over HTTPS
function grpcWebCall(
    root: protobuf.Root,
    servicePath: string,
    method: string,
    requestType: string,
    responseType: string,
    requestData: object
): Promise<object> {
    const ReqType = root.lookupType(requestType);
    const ResType = root.lookupType(responseType);

    const encoded = ReqType.encode(ReqType.create(requestData)).finish();

    // gRPC-Web frame: 1-byte flags (0 = no compression) + 4-byte big-endian length + body
    const frame = Buffer.alloc(5 + encoded.length);
    frame.writeUInt8(0, 0);
    frame.writeUInt32BE(encoded.length, 1);
    Buffer.from(encoded).copy(frame, 5);

    return new Promise((resolve, reject) => {
        const req = https.request(
            {
                hostname: HOST,
                port: 443,
                path: `/${ACCESS_TOKEN}/${servicePath}/${method}`,
                method: 'POST',
                headers: {
                    'content-type': 'application/grpc-web+proto',
                    'x-grpc-web': '1',
                    'accept': 'application/grpc-web+proto',
                    'content-length': frame.length,
                },
            },
            (res) => {
                console.log('HTTP status  :', res.statusCode);
                console.log('Content-Type :', res.headers['content-type']);

                const chunks: Buffer[] = [];
                res.on('data', (chunk: Buffer) => chunks.push(chunk));
                res.on('end', () => {
                    const data = Buffer.concat(chunks);
                    if (data.length < 5) {
                        return reject(new Error(`Short response: ${data.toString('hex')}`));
                    }

                    const flags = data.readUInt8(0);
                    const length = data.readUInt32BE(1);

                    // Trailer frame (flags bit 7 set) contains gRPC status metadata
                    if (flags & 0x80) {
                        const trailer = data.slice(5).toString();
                        return reject(new Error(`gRPC-Web error trailer: ${trailer}`));
                    }

                    const responseBytes = data.slice(5, 5 + length);
                    const response = ResType.decode(responseBytes);
                    resolve(ResType.toObject(response, { longs: String, enums: String, defaults: true }));
                });
            }
        );

        req.on('error', reject);
        req.write(frame);
        req.end();
    });
}

async function main() {
    const root = new protobuf.Root();
    root.resolvePath = (_origin: string, target: string) =>
        target.startsWith('/') ? target : path.join(PROTO_ROOT, target);
    await root.load(path.join(PROTO_ROOT, 'sui/rpc/v2/ledger_service.proto'), { keepCase: true });

    console.log('Fetching service info...\n');
    const info = await grpcWebCall(
        root,
        'sui.rpc.v2.LedgerService',
        'GetServiceInfo',
        'sui.rpc.v2.GetServiceInfoRequest',
        'sui.rpc.v2.GetServiceInfoResponse',
        {}
    );
    console.log('Service Info:', JSON.stringify(info, null, 2));
}

main().catch(console.error);

```
{% endcode %}

#### Response

{% code overflow="wrap" %}
```bash
Fetching SUI balance...

Balance: {
  "balance": {
    "coin_type": "0x0000000000000000000000000000000000000000000000000000000000000002::sui::SUI",
    "balance": "1035000074",
    "coin_balance": "1035000074"
  }
}
```
{% endcode %}

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
