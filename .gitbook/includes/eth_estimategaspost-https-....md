---
title: eth_estimateGasPOST https:/...
---

````
eth_estimateGas

POST https://arb-mainnet.g.alchemy.com/v2/{apiKey}
Content-Type: application/json

Generates and returns an estimate of how much gas is necessary to allow the transaction to complete.

Reference: https://alchemy.com/docs/chains/arbitrum/arbitrum-api-endpoints/eth-estimate-gas

## OpenAPI Specification

```yaml
openapi: 3.1.1
info:
  title: eth_estimateGas
  version: eth_estimateGas
paths:
  /{apiKey}:
    post:
      operationId: eth-estimate-gas
      summary: eth_estimateGas
      description: >-
        Generates and returns an estimate of how much gas is necessary to allow
        the transaction to complete.
      tags:
        - []
      parameters:
        - name: apiKey
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: >-
            The estimated amount of gas required for the transaction, as a
            hexadecimal string.
          content:
            application/json:
              schema:
                type: string
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                Transaction:
                  $ref: '#/components/schemas/eth_estimateGas_Param_Transaction'
                Block:
                  $ref: '#/components/schemas/eth_estimateGas_Param_Block'
              required:
                - Transaction
components:
  schemas:
    EthEstimateGasParamTransactionAccessListItems:
      type: object
      properties:
        address:
          type: string
        storageKeys:
          type: array
          items:
            type: string
    eth_estimateGas_Param_Transaction:
      type: object
      properties:
        type:
          type: string
        nonce:
          type: string
        to:
          type:
            - string
            - 'null'
        from:
          type: string
        gas:
          type: string
        value:
          type: string
        input:
          type: string
        gasPrice:
          type: string
        maxPriorityFeePerGas:
          type: string
        maxFeePerGas:
          type: string
        maxFeePerBlobGas:
          type: string
        accessList:
          type: array
          items:
            $ref: '#/components/schemas/EthEstimateGasParamTransactionAccessListItems'
        blobVersionedHashes:
          type: array
          items:
            type: string
        blobs:
          type: array
          items:
            type: string
        chainId:
          type: string
    EthEstimateGasParamBlock1:
      type: string
      enum:
        - value: earliest
        - value: finalized
        - value: safe
        - value: latest
        - value: pending
    eth_estimateGas_Param_Block:
      oneOf:
        - type: string
        - $ref: '#/components/schemas/EthEstimateGasParamBlock1'

```

## SDK Code Examples

```python
import requests

url = "https://arb-mainnet.g.alchemy.com/v2/:apiKey"

payload = [
    {
        "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "to": "0x44aa93095d6749a706051658b970b941c72c1d53",
        "value": "0x1"
    }
]
headers = {"Content-Type": "application/json"}

response = requests.post(url, json=payload, headers=headers)

print(response.json())
```

```javascript
const url = 'https://arb-mainnet.g.alchemy.com/v2/:apiKey';
const options = {
  method: 'POST',
  headers: {'Content-Type': 'application/json'},
  body: '[{"from":"0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045","to":"0x44aa93095d6749a706051658b970b941c72c1d53","value":"0x1"}]'
};

try {
  const response = await fetch(url, options);
  const data = await response.json();
  console.log(data);
} catch (error) {
  console.error(error);
}
```

```go
package main

import (
	"fmt"
	"strings"
	"net/http"
	"io"
)

func main() {

	url := "https://arb-mainnet.g.alchemy.com/v2/:apiKey"

	payload := strings.NewReader("[\n  {\n    \"from\": \"0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045\",\n    \"to\": \"0x44aa93095d6749a706051658b970b941c72c1d53\",\n    \"value\": \"0x1\"\n  }\n]")

	req, _ := http.NewRequest("POST", url, payload)

	req.Header.Add("Content-Type", "application/json")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := io.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://arb-mainnet.g.alchemy.com/v2/:apiKey")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request.body = "[\n  {\n    \"from\": \"0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045\",\n    \"to\": \"0x44aa93095d6749a706051658b970b941c72c1d53\",\n    \"value\": \"0x1\"\n  }\n]"

response = http.request(request)
puts response.read_body
```

```java
HttpResponse<String> response = Unirest.post("https://arb-mainnet.g.alchemy.com/v2/:apiKey")
  .header("Content-Type", "application/json")
  .body("[\n  {\n    \"from\": \"0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045\",\n    \"to\": \"0x44aa93095d6749a706051658b970b941c72c1d53\",\n    \"value\": \"0x1\"\n  }\n]")
  .asString();
```

```php
<?php

$client = new \GuzzleHttp\Client();

$response = $client->request('POST', 'https://arb-mainnet.g.alchemy.com/v2/:apiKey', [
  'body' => '[
  {
    "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
    "to": "0x44aa93095d6749a706051658b970b941c72c1d53",
    "value": "0x1"
  }
]',
  'headers' => [
    'Content-Type' => 'application/json',
  ],
]);

echo $response->getBody();
```

```csharp
var client = new RestClient("https://arb-mainnet.g.alchemy.com/v2/:apiKey");
var request = new RestRequest(Method.POST);
request.AddHeader("Content-Type", "application/json");
request.AddParameter("application/json", "[\n  {\n    \"from\": \"0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045\",\n    \"to\": \"0x44aa93095d6749a706051658b970b941c72c1d53\",\n    \"value\": \"0x1\"\n  }\n]", ParameterType.RequestBody);
IRestResponse response = client.Execute(request);
```

```swift
import Foundation

let headers = ["Content-Type": "application/json"]
let parameters = [
  [
    "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
    "to": "0x44aa93095d6749a706051658b970b941c72c1d53",
    "value": "0x1"
  ]
] as [String : Any]

let postData = JSONSerialization.data(withJSONObject: parameters, options: [])

let request = NSMutableURLRequest(url: NSURL(string: "https://arb-mainnet.g.alchemy.com/v2/:apiKey")! as URL,
                                        cachePolicy: .useProtocolCachePolicy,
                                    timeoutInterval: 10.0)
request.httpMethod = "POST"
request.allHTTPHeaderFields = headers
request.httpBody = postData as Data

let session = URLSession.shared
let dataTask = session.dataTask(with: request as URLRequest, completionHandler: { (data, response, error) -> Void in
  if (error != nil) {
    print(error as Any)
  } else {
    let httpResponse = response as? HTTPURLResponse
    print(httpResponse)
  }
})

dataTask.resume()
```
````
