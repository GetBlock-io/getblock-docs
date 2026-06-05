---
description: >-
  GetBlock provides fast and reliable access to Sui gRPC nodes via  gRPC API.
  Connect to the SUI network without running your own infrastructure.
---

# Sui (SUI)

Sui is a Layer 1 blockchain built by Mysten Labs, designed from the ground up for low-latency, high-throughput applications. It uses the Move programming language — a Rust-based, asset-oriented smart contract language with built-in safety guarantees — and an object-centric data model where every on-chain entity is an object with ownership properties.&#x20;

The network achieves sub-second finality (\~400ms average) through Mysticeti consensus (which evolved from Narwhal and Bullshark) and processes independent transactions in parallel, with a theoretical throughput of 100,000+ TPS.&#x20;

Sui's RPC surface is built around gRPC with Protocol Buffers — strongly typed, type-safe, bandwidth-efficient, and supporting server-side streaming for real-time data delivery. Methods are organized into seven services: `LedgerService`, `StateService`, `TransactionExecutionService`, `MovePackageService`, `NameService`, `SignatureVerificationService`, and `SubscriptionService`.

### Key Features

* **gRPC Native**: Strongly typed, Protocol Buffers-based RPC with automatic client code generation for TypeScript, Go, Rust, Python, and more — significantly faster and lighter than JSON-RPC
* **Object-Centric Model**: Everything on Sui is an object with explicit ownership — enables parallel transaction execution for independent operations
* **Move Programming Language**: Rust-based smart contract language with linear types, resource safety, and verifier checks built into the toolchain
* **Mysticeti Consensus**: Sub-second finality (\~400ms average) via the latest evolution of Narwhal-Bullshark DAG-based consensus
* **Parallel Transaction Execution**: Transactions that touch disjoint objects execute in parallel — horizontal throughput scaling
* **SUI Native Token**: 9 decimals — smallest unit MIST. Used for gas, staking, and governance
* **Sponsored Transactions**: Gas payment abstraction — third parties can pay gas on behalf of users for better UX
* **SuiNS**: Native name service for human-readable addresses (`example.sui` resolves to `0x...`)
* **zkLogin**: Native account abstraction using OAuth credentials (Google, Apple, Facebook, etc.) with zero-knowledge proofs
* **Dynamic NFTs**: Native support for evolving, composable digital assets through Move's resource model
* **Field Masks**: gRPC `read_mask` parameter lets clients request only the fields they need — major bandwidth and latency win for indexers and explorers
* **Server-Side Streaming**: `SubscribeCheckpoints` provides real-time checkpoint updates over a single long-lived gRPC stream — replaces the deprecated JSON-RPC WebSocket subscriptions

{% hint style="info" %}
_**TECHNICAL DISCLAIMER: AUTHORITATIVE gRPC SPECIFICATION**_

_GetBlock’s Sui API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for the Sui gRPC interface — including service definitions, message types, enums, and scalar types — is maintained by Mysten Labs and the Sui Foundation and published at_ [_docs.sui.io/references/fullnode-protocol_](https://docs.sui.io/references/fullnode-protocol)_. The official `.proto` source files are hosted at_ [_github.com/MystenLabs/sui-apis_](https://github.com/MystenLabs/sui-apis)_. For Move language details, the object model, Mysticeti consensus, and SDK references, consult the_ [_official Sui documentation_](https://docs.sui.io/)_._
{% endhint %}

{% hint style="warning" %}
**JSON-RPC IS DEPRECATED — SUNSET END OF JULY 2026**

_Sui's JSON-RPC interface is being deprecated and will be fully deactivated at the end of July 2026, in line with the Sui Foundation's network-wide deprecation timeline. **Migrate existing integrations to gRPC (recommended for indexers and backend services) or GraphQL (recommended for frontends and analytics).** GetBlock continues to support JSON-RPC throughout the transition for compatibility, but new integrations should target gRPC directly. This documentation focuses exclusively on the gRPC API._

_For JSON-RPC method mappings, decision criteria, and common migration gotchas, see the_ [_official JSON-RPC Migration Guide_](https://docs.sui.io/references/sui-api)_._
{% endhint %}

## Network Information

| Property                | Value                                                                        |
| ----------------------- | ---------------------------------------------------------------------------- |
| Network Name            | Sui Mainnet                                                                  |
| Chain ID                | 101 (mainnet)                                                                |
| Native Currency         | SUI                                                                          |
| Decimals                | 9 (1 SUI = 1,000,000,000 MIST)                                               |
| Block Time / Checkpoint | \~400 ms average finality (Mysticeti consensus)                              |
| Consensus               | Mysticeti (DAG-based BFT, single-slot finality)                              |
| VM                      | Move (Rust-based smart contract language)                                    |
| Data Model              | Object-centric (not account-based)                                           |
| Address Format          | 32-byte hex `0x...` (different from Ethereum's 20-byte)                      |
| Block Explorer          | [suiscan.xyz](https://suiscan.xyz/), [suivision.xyz](https://suivision.xyz/) |
| Protocol                | gRPC with Protocol Buffers (HTTP/2) — gRPC-Web also supported                |
| Port                    | 443 (TLS)                                                                    |

## Base URL

All Sui gRPC methods follow the pattern:

```bash
https://go.getblock.io/<ACCESS-TOKEN>/sui.rpc.v2.<ServiceName>/<MethodName>
```

For example, to call `GetServiceInfo` on the `LedgerService`:

```bash
https://go.getblock.io/<ACCESS-TOKEN>/sui.rpc.v2.LedgerService/GetServiceInfo
```

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="New York, USA" %}
```bash
https://go.getblock.us/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

For real-time `SubscribeCheckpoints` streaming, the same base URL is used over the same gRPC connection — Sui's subscription API is a server-side streaming RPC, **not** a WebSocket subscription.

## Supported Networks

| Network | gRPC | Frankfurt, Germany | New York, USA |
| ------- | ---- | ------------------ | ------------- |
| Mainnet | ✅    | ✅                  | ✅             |

## Quickstart

In this section, you will learn how to make your first gRPC call against Sui mainnet using **grpcurl** — the standard CLI tool for gRPC, which works against the official Sui proto files. You can also use generated TypeScript / Python / Rust clients (see the SDK Integration section on every endpoint page).

Before you begin, you must have installed:

* [grpcurl](https://github.com/fullstorydev/grpcurl) (`brew install grpcurl` on macOS, or download from the GitHub releases page)
* The Sui proto files from [github.com/MystenLabs/sui-apis](https://github.com/MystenLabs/sui-apis)

{% tabs %}
{% tab title="grpcurl (CLI)" %}
{% stepper %}
{% step %}
### Clone the Sui proto files

```bash
git clone https://github.com/MystenLabs/sui-apis.git
cd sui-apis
```
{% endstep %}

{% step %}
### Make your first gRPC call

```bash
grpcurl \
  -import-path proto \
  -proto sui/rpc/v2/ledger_service.proto \
  -H "x-grpc-web: 1" \
  -d '{}' \
  go.getblock.io:443/<ACCESS-TOKEN> \
  sui.rpc.v2.LedgerService/GetServiceInfo
```

Replace `<ACCESS-TOKEN>` with your actual GetBlock access token.
{% endstep %}

{% step %}
### Expected response

```json
{
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

`chain: "mainnet"` confirms you are connected to Sui mainnet. The `checkpoint_height` is the current chain tip.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="TypeScript (@grpc/grpc-js)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir sui-grpc-quickstart
cd sui-grpc-quickstart
npm init --yes
npm install @grpc/grpc-js @grpc/proto-loader
npm install -D typescript ts-node @types/node
npx tsc --init
```
{% endstep %}

{% step %}
### Download proto files

```bash
git clone https://github.com/MystenLabs/sui-apis.git protos
```
{% endstep %}

{% step %}
### Create `index.ts`

{% code title="index.ts" %}
```typescript
import * as grpc from '@grpc/grpc-js';
import * as protoLoader from '@grpc/proto-loader';
import * as path from 'path';

const PROTO_PATH = path.join(__dirname, 'protos/proto/sui/rpc/v2/ledger_service.proto');
const ACCESS_TOKEN = '<ACCESS-TOKEN>';

const packageDef = protoLoader.loadSync(PROTO_PATH, {
    includeDirs: [path.join(__dirname, 'protos/proto')],
    keepCase: true,
    longs: String,
    enums: String,
    defaults: true,
});

const proto = grpc.loadPackageDefinition(packageDef) as any;
const LedgerService = proto.sui.rpc.v2.LedgerService;

// Use credentials with metadata for the access token
const metadata = new grpc.Metadata();
metadata.add('authorization', `Bearer ${ACCESS_TOKEN}`);

const client = new LedgerService(
    'go.getblock.io:443',
    grpc.credentials.createSsl()
);

client.GetServiceInfo({}, metadata, (err: any, response: any) => {
    if (err) {
        console.error('Error:', err);
        return;
    }
    console.log('Service Info:', JSON.stringify(response, null, 2));
});
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
{% endstep %}

{% step %}
### Run

```bash
npx ts-node index.ts
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

### More Examples

#### Get Balance (StateService)

To query balances, use the `StateService`:

{% tabs %}
{% tab title="Request" %}
{% code title="getBalance.ts" overflow="wrap" %}
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
{% endtab %}

{% tab title="Response" %}
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
{% endtab %}
{% endtabs %}

### Available API Methods

Sui's gRPC API is organized into **7 services** with a total of **22 methods**. Field masks (`read_mask: {paths: [...]}`) let you request only specific response fields — use `["*"]` for all fields.

#### LedgerService (7 methods)

Core blockchain queries — service info, objects, transactions, checkpoints, epochs.

| Method               | Description                                            |
| -------------------- | ------------------------------------------------------ |
| GetServiceInfo       | Returns node info — chain ID, epoch, checkpoint height |
| GetObject            | Returns object data by object ID                       |
| BatchGetObjects      | Returns multiple objects by IDs in a single call       |
| GetTransaction       | Returns a transaction by its digest                    |
| BatchGetTransactions | Returns multiple transactions in a single call         |
| GetCheckpoint        | Returns a checkpoint by sequence number or digest      |
| GetEpoch             | Returns information about a specific epoch             |

#### StateService (5 methods)

Account state and balance queries — owned objects, dynamic fields, coin balances.

| Method            | Description                                             |
| ----------------- | ------------------------------------------------------- |
| ListDynamicFields | Returns dynamic fields of a parent object               |
| ListOwnedObjects  | Returns objects owned by an address                     |
| GetCoinInfo       | Returns coin metadata (symbol, decimals, supply)        |
| GetBalance        | Returns balance for an address and a specific coin type |
| ListBalances      | Returns all coin balances for an address                |

#### TransactionExecutionService (2 methods)

Transaction submission and simulation.

| Method              | Description                                                                          |
| ------------------- | ------------------------------------------------------------------------------------ |
| ExecuteTransaction  | Submits and executes a signed transaction                                            |
| SimulateTransaction | Dry-runs a transaction without execution (for cost estimation and pre-flight checks) |

#### MovePackageService (4 methods)

Introspection of Move packages and their types/functions.

| Method              | Description                                              |
| ------------------- | -------------------------------------------------------- |
| GetPackage          | Returns Move package metadata and modules                |
| GetDatatype         | Returns the definition of a Move struct or enum          |
| GetFunction         | Returns the signature of a Move function                 |
| ListPackageVersions | Returns all versions of a package (for upgrade tracking) |

#### NameService (2 methods)

SuiNS (Sui Name Service) — human-readable addresses.

| Method            | Description                                              |
| ----------------- | -------------------------------------------------------- |
| LookupName        | Resolves a SuiNS name (e.g. `example.sui`) to an address |
| ReverseLookupName | Resolves an address to its primary SuiNS name            |

#### SignatureVerificationService (1 method)

| Method          | Description                                  |
| --------------- | -------------------------------------------- |
| VerifySignature | Verifies a `UserSignature` against a message |

#### SubscriptionService (1 method, server-side streaming)

| Method               | Description                                                                                                                                     |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| SubscribeCheckpoints | Streams checkpoints in order, starting from the latest executed checkpoint as seen by the server. Server-side streaming RPC — **not** WebSocket |

### Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

### See Also

* [Sui gRPC Methods Reference](https://docs.sui.io/references/fullnode-protocol) (canonical)
* [Sui gRPC Message Definitions](https://docs.sui.io/references/fullnode-protocol-messages)
* [Sui gRPC Enum and Scalar Types](https://docs.sui.io/references/fullnode-protocol-types)
* [Official Sui Proto Files (sui-apis)](https://github.com/MystenLabs/sui-apis)
* [JSON-RPC to gRPC Migration Guide](https://docs.sui.io/references/sui-api)
* [Sui Documentation Portal](https://docs.sui.io/)
* [Sui TypeScript SDK](https://sdk.mystenlabs.com/typescript)
* [Sui Rust SDK](https://docs.sui.io/references/rust-sdk)
* [Suiscan Explorer](https://suiscan.xyz/)
* [SuiVision Explorer](https://suivision.xyz/)
* [Protocol Buffers Documentation](https://protobuf.dev/)
* [grpcurl CLI tool](https://github.com/fullstorydev/grpcurl)
* [Move Language Documentation](https://move-language.github.io/move/)
* [Sui Bridge](https://bridge.sui.io/)
* [SuiNS](https://suins.io/)
