---
description: >-
  Example code for the GetAccountResource gRPC method. Complete guide on how to
  use GetAccountResource gRPC method in GetBlock Web3 documentation.
---

# GetAccountResource - Tron

This method returns an account's resource state: Energy and Bandwidth limits, current usage, and the total network resource pools.

## Service

This method is served by the `protocol.Wallet` service.

## Method

```protobuf
rpc GetAccountResource (Account) returns (AccountResourceMessage)
```

## Request Message

`Account`

| Field   | Type  | Description                                |
| ------- | ----- | ------------------------------------------ |
| address | bytes | Account address as raw bytes (41-prefixed) |

## Request

{% tabs %}
{% tab title="grpcurl" %}
{% code overflow="wrap" %}
```bash
grpcurl -H 'x-api-key: <ACCESS-TOKEN>' \
  -d '{"address": "41f0cc5a2a84cd0f68ed1667070934542d673acbd8"}' \
  shared.eu-central-1.getblock.io:443 protocol.Wallet/GetAccountResource
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import grpc
# Stubs generated from java-tron's api.proto with grpcio-tools
from api import api_pb2_grpc
from core import Contract_pb2, Tron_pb2

creds = grpc.ssl_channel_credentials()
channel = grpc.secure_channel('shared.eu-central-1.getblock.io:443', creds)
metadata = [('x-api-key', '<ACCESS-TOKEN>')]
stub = api_pb2_grpc.WalletStub(channel)

response = stub.GetAccountResource(Tron_pb2.Account(address=bytes.fromhex('41f0cc5a2a84cd0f68ed1667070934542d673acbd8')), metadata=metadata)
print(response)
```
{% endcode %}
{% endtab %}

{% tab title="Go" %}
{% code title="example.go" %}
```go
conn, _ := grpc.Dial(
    "shared.eu-central-1.getblock.io:443",
    grpc.WithTransportCredentials(credentials.NewTLS(&tls.Config{})),
)
defer conn.Close()

client := api.NewWalletClient(conn)
ctx := metadata.AppendToOutgoingContext(
    context.Background(), "x-api-key", "<ACCESS-TOKEN>")

resp, _ := client.GetAccountResource(ctx, &core.Account{Address: addrBytes})
fmt.Println(resp)
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Message

`AccountResourceMessage`

| Field            | Type  | Description                               |
| ---------------- | ----- | ----------------------------------------- |
| freeNetLimit     | int64 | Free Bandwidth available to every account |
| NetLimit         | int64 | Bandwidth available from staked TRX       |
| EnergyLimit      | int64 | Energy available from staked TRX          |
| EnergyUsed       | int64 | Energy consumed in the current window     |
| TotalEnergyLimit | int64 | Total Energy pool across the network      |

## Use Cases

* **Fee Preflight**: Check available Energy before a contract call
* **Resource Planning**: Decide how much TRX to stake
* **Transaction Sizing**: Confirm Bandwidth covers a transaction
* **Monitoring**: Track resource consumption

## Error Handling

| Code | Status            | Description                                                 |
| ---- | ----------------- | ----------------------------------------------------------- |
| 3    | INVALID\_ARGUMENT | A request field is missing or malformed                     |
| 5    | NOT\_FOUND        | The requested account, block, or transaction does not exist |
| 16   | UNAUTHENTICATED   | The access token is missing or invalid                      |
| 13   | INTERNAL          | The node failed to process the request                      |
