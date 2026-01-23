---
description: >-
  A step-by-step guide to subscribing to Base Flashblocks and building trading
  applications around ultra-fast blockchain data.
icon: ear-waveform
---

# How To Build a Base Flashblocks Listener in Go

Traditional blockchain confirmations are slow. On Ethereum Layer 2 networks, such as Base, blocks are produced every 2 seconds. While this is already faster than Ethereum's \~12-second blocks, it's still an eternity for time-sensitive applications like trading bots, real-time games, or DeFi protocols where milliseconds matter.

Flashblocks break the limitation of traditional blocks by creating mini-blocks every 200 milliseconds, providing users with instant transaction status while still retaining cryptographic security.&#x20;

_**In this guide, you'll learn what base flashblocks are, their importance, data structure and step-by-step instructions on how to build a base flashblocks listener.**_&#x20;

### What Are Base Flashblocks?

Flashblocks are sub-blocks (or "mini-blocks") streamed every **200 milliseconds** — that's 10x faster than Base's standard 2-second block time. Developed in collaboration with Flashbots and launched on Base Mainnet in July 2025, Flashblocks provide **preconfirmations**: ultra-fast signals that arrive before the next full block is sealed.

Here's how it works:

* **Standard Base blocks**: Created every 2 seconds, containing all transactions
* **Flashblocks**: 10 sub-blocks per full block, streamed every 200ms
* Each Flashblock contains \~10% of the transactions (by gas) of a regular block
* A series of Flashblocks can be combined to recreate a full block

{% hint style="info" %}
Think of it like texting vs. email. Traditional blocks are like writing an entire email, proofreading it, and hitting send. Flashblocks are like texting — you send each line instantly and keep going.
{% endhint %}

### Importance of Flashblocks for Trading

For trading applications, Flashblocks unlock several critical advantages:

1. **Near-instant transaction feedback**: Know within 200ms if your transaction was included
2. **Front-running prevention**: Once a Flashblock is built and broadcast, its transaction ordering is locked. Later-arriving transactions with higher fees can't jump ahead
3. **Real-time state updates**: See balance changes, liquidity shifts, and price movements as they happen
4. **CEX-like experience on DEXs**: Trading feels instant, not sluggish

### What You're Building

In this tutorial, you'll build a Go application that:

* Connects to the Base Flashblocks WebSocket stream
* Handles both plain JSON and Brotli-compressed messages
* Parses Flashblock data structures (transactions, receipts, balance updates)
* Displays real-time blockchain activity

This forms the foundation for building trading bots, monitoring tools, or any application that needs ultra-low-latency blockchain data.

## Understanding the Flashblock Data Structure

Before diving into code, let's understand what data you'll receive. Each Flashblock message contains three main components:

### 1. The Root Structure

```go
type Flashblock struct {
    Diff     BlockDiff `json:"diff"`      // Block differences/updates
    Index    int       `json:"index"`     // Flashblock index (0-9 within a block)
    Metadata Metadata  `json:"metadata"`  // Block metadata
}
```

The `Index` field tells you which of the 10 Flashblocks within a full block you're receiving (0 through 9).

### 2. BlockDiff — The Block Changes

```go
type BlockDiff struct {
    BlobGasUsed     string   `json:"blob_gas_used"`
    BlockHash       string   `json:"block_hash"`
    GasUsed         string   `json:"gas_used"`
    LogsBloom       string   `json:"logs_bloom"`
    ReceiptsRoot    string   `json:"receipts_root"`
    StateRoot       string   `json:"state_root"`
    Transactions    []string `json:"transactions"`
    Withdrawals     []any    `json:"withdrawals"`
    WithdrawalsRoot string   `json:"withdrawals_root"`
}
```

Key fields for trading applications:

* **`Transactions`**: Array of raw transaction data included in this Flashblock
* **`StateRoot`**: Cryptographic proof of the blockchain state after these transactions
* **`GasUsed`**: Total gas consumed by transactions in this Flashblock

### 3. Metadata — Balances and Receipts

```go
type Metadata struct {
    BlockNumber        uint64             `json:"block_number"`
    NewAccountBalances map[string]string  `json:"new_account_balances"`
    Receipts           map[string]Receipt `json:"receipts"`
}
```

This is where the trading gold is:

* **`BlockNumber`**: The full block number this Flashblock belongs to
* **`NewAccountBalances`**: Map of addresses to their updated ETH balances
* **`Receipts`**: Transaction receipts with execution results and event logs

### 4. Receipts — Transaction Results

```go
type Receipt struct {
    Eip1559 *ReceiptData `json:"Eip1559,omitempty"`
    Legacy  *ReceiptData `json:"Legacy,omitempty"`
}

type ReceiptData struct {
    CumulativeGasUsed string `json:"cumulativeGasUsed"`
    Logs              []Log  `json:"logs"`
    Status            string `json:"status"`
}

type Log struct {
    Address string   `json:"address"`
    Data    string   `json:"data"`
    Topics  []string `json:"topics"`
}
```

Receipts come in two flavors: EIP-1559 (modern) and Legacy. The `Logs` array contains event emissions — crucial for detecting swaps, transfers, and other contract events.

## Prerequisites

Before we begin, make sure you have:

* **Go 1.21+** installed ([download here](https://go.dev/dl/))
* Basic understanding of WebSockets and blockchain concepts
* A terminal/command line environment

### Dependencies

This application uses two external packages:

1. [WebSocket client library](https://github.com/gorilla/websocket)
2. [Brotli decompression](https://github.com/andybalholm/brotli) (Flashblocks are compressed)

## Project Setup

1. Create a new directory for your project and initialize the Go module:

```bash
mkdir flashblocks-listener
cd flashblocks-listener
go mod init flashblocks-listener
```

2. Install the required dependencies:

```bash
go get github.com/gorilla/websocket
go get github.com/andybalholm/brotli
```

3. Your `go.mod` file should look like this:

```go
module flashblocks-listener

go 1.23.0

require (
    github.com/andybalholm/brotli v1.2.0
    github.com/gorilla/websocket v1.5.3
)
```

## Building the Listener

1. Create `main.go` and start with the package and imports:

```go
package main

import (
    "bytes"
    "encoding/json"
    "flag"
    "fmt"
    "io"
    "log"
    "os"
    "os/signal"
    "syscall"

    "github.com/andybalholm/brotli"
    "github.com/gorilla/websocket"
)
```

This imports standard library packages for JSON handling, logging, and signal management, plus our two external dependencies.



2. Define WebSocket endpoints and the data structures described earlier:

```go
const (
    mainnetWSURL = "wss://mainnet.flashblocks.base.org/ws"
    sepoliaWSURL = "wss://sepolia.flashblocks.base.org/ws"
)
```

Now define all the data structures:

```go
// Flashblock represents the root structure of a flashblock message
type Flashblock struct {
    Diff     BlockDiff `json:"diff"`
    Index    int       `json:"index"`
    Metadata Metadata  `json:"metadata"`
}

// BlockDiff contains the block differences/updates
type BlockDiff struct {
    BlobGasUsed     string   `json:"blob_gas_used"`
    BlockHash       string   `json:"block_hash"`
    GasUsed         string   `json:"gas_used"`
    LogsBloom       string   `json:"logs_bloom"`
    ReceiptsRoot    string   `json:"receipts_root"`
    StateRoot       string   `json:"state_root"`
    Transactions    []string `json:"transactions"`
    Withdrawals     []any    `json:"withdrawals"`
    WithdrawalsRoot string   `json:"withdrawals_root"`
}

// Metadata contains block metadata including balances and receipts
type Metadata struct {
    BlockNumber        uint64             `json:"block_number"`
    NewAccountBalances map[string]string  `json:"new_account_balances"`
    Receipts           map[string]Receipt `json:"receipts"`
}

// Receipt represents a transaction receipt (can be EIP-1559 or Legacy)
type Receipt struct {
    Eip1559 *ReceiptData `json:"Eip1559,omitempty"`
    Legacy  *ReceiptData `json:"Legacy,omitempty"`
}

// GetData returns the receipt data regardless of type
func (r *Receipt) GetData() *ReceiptData {
    if r.Eip1559 != nil {
        return r.Eip1559
    }
    return r.Legacy
}

// GetType returns the receipt type as a string
func (r *Receipt) GetType() string {
    if r.Eip1559 != nil {
        return "EIP-1559"
    }
    if r.Legacy != nil {
        return "Legacy"
    }
    return "Unknown"
}

// ReceiptData contains the actual receipt information
type ReceiptData struct {
    CumulativeGasUsed string `json:"cumulativeGasUsed"`
    Logs              []Log  `json:"logs"`
    Status            string `json:"status"`
}

// Log represents an event log
type Log struct {
    Address string   `json:"address"`
    Data    string   `json:"data"`
    Topics  []string `json:"topics"`
}
```

The `GetData()` and `GetType()` helper methods on `Receipt` let us work with receipts without worrying about whether they're EIP-1559 or Legacy format.

3. Implement the main routine: flag parsing, selecting the endpoint, connecting via WebSocket, and setting up message processing and graceful shutdown.

{% code overflow="wrap" %}
```go
func main() {
    // Parse command-line flags
    network := flag.String("network", "mainnet", "Network to connect to: mainnet or sepolia")
    flag.Parse()

    // Select the appropriate WebSocket URL
    var wsURL string
    switch *network {
    case "mainnet":
        wsURL = mainnetWSURL
    case "sepolia":
        wsURL = sepoliaWSURL
    default:
        log.Fatalf("Invalid network: %s. Use 'mainnet' or 'sepolia'", *network)
    }

    log.Printf("Connecting to Base flashblocks on %s: %s", *network, wsURL)

    // Establish WebSocket connection
    conn, _, err := websocket.DefaultDialer.Dial(wsURL, nil)
    if err != nil {
        log.Fatalf("Failed to connect to WebSocket: %v", err)
    }
    defer conn.Close()

    log.Println("Connected! Listening for flashblocks...")

    // Set up graceful shutdown
    sigChan := make(chan os.Signal, 1)
    signal.Notify(sigChan, syscall.SIGINT, syscall.SIGTERM)

    done := make(chan struct{})

    // Message processing goroutine
    go func() {
        defer close(done)
        for {
            messageType, message, err := conn.ReadMessage()
            if err != nil {
                if websocket.IsCloseError(err, websocket.CloseNormalClosure, websocket.CloseGoingAway) {
                    log.Println("WebSocket closed normally")
                    return
                }
                log.Printf("Error reading message: %v", err)
                return
            }

            switch messageType {
            case websocket.TextMessage:
                handleFlashblockJSON(message)
            case websocket.BinaryMessage:
                decoded, err := decodeBrotli(message)
                if err != nil {
                    log.Printf("Error decoding Brotli: %v", err)
                    log.Printf("Raw binary message length: %d bytes", len(message))
                    continue
                }
                handleFlashblockJSON(decoded)
            default:
                log.Printf("Received unknown message type: %d", messageType)
            }
        }
    }()

    // Wait for shutdown signal or connection close
    select {
    case <-done:
        log.Println("Connection closed")
    case sig := <-sigChan:
        log.Printf("Received signal %v, shutting down...", sig)
        err := conn.WriteMessage(websocket.CloseMessage, 
            websocket.FormatCloseMessage(websocket.CloseNormalClosure, ""))
        if err != nil {
            log.Printf("Error sending close message: %v", err)
        }
    }
}
```
{% endcode %}

This covers:

* Flag parsing for `-network`
* WebSocket connection via gorilla/websocket
* Signal handling for graceful shutdown
* Message loop reading text or binary (Brotli) messages



4. Flashblock messages are typically Brotli-compressed. Decompress them with:

```go
func decodeBrotli(data []byte) ([]byte, error) {
    reader := brotli.NewReader(bytes.NewReader(data))
    return io.ReadAll(reader)
}
```

5. Unmarshal JSON into structs and print a readable summary.

```go
func handleFlashblockJSON(data []byte) {
    var flashblock Flashblock
    if err := json.Unmarshal(data, &flashblock); err != nil {
        log.Printf("Error parsing flashblock JSON: %v", err)
        log.Printf("Raw data: %s", string(data))
        return
    }

    printFlashblock(&flashblock)
}
```

Pretty-print the flashblock:

```go
func printFlashblock(fb *Flashblock) {
    fmt.Println("═══════════════════════════════════════════════════════════════")
    fmt.Printf("FLASHBLOCK #%d | Block: %d | Hash: %s\n",
        fb.Index,
        fb.Metadata.BlockNumber,
        truncateHash(fb.Diff.BlockHash))
    fmt.Println("═══════════════════════════════════════════════════════════════")

    fmt.Printf("  Gas Used:       %s\n", fb.Diff.GasUsed)
    fmt.Printf("  Blob Gas Used:  %s\n", fb.Diff.BlobGasUsed)
    fmt.Printf("  State Root:     %s\n", truncateHash(fb.Diff.StateRoot))
    fmt.Printf("  Receipts Root:  %s\n", truncateHash(fb.Diff.ReceiptsRoot))

    fmt.Printf("\n  Transactions: %d\n", len(fb.Diff.Transactions))
    for i, tx := range fb.Diff.Transactions {
        fmt.Printf("    [%d] %s...\n", i, truncateHash(tx))
    }

    fmt.Printf("\n  Account Balance Updates: %d\n", len(fb.Metadata.NewAccountBalances))

    fmt.Printf("\n  Receipts: %d\n", len(fb.Metadata.Receipts))
    receiptCount := 0
    for txHash, receipt := range fb.Metadata.Receipts {
        if receiptCount >= 3 {
            fmt.Printf("    ... and %d more receipts\n", len(fb.Metadata.Receipts)-3)
            break
        }
        data := receipt.GetData()
        if data != nil {
            fmt.Printf("    [%s] %s - Status: %s, Logs: %d\n",
                receipt.GetType(),
                truncateHash(txHash),
                data.Status,
                len(data.Logs))
        }
        receiptCount++
    }

    fmt.Println()
}

func truncateHash(hash string) string {
    if len(hash) <= 20 {
        return hash
    }
    return hash[:10] + "..." + hash[len(hash)-8:]
}
```

This output shows:

* Flashblock index (0-9)
* Block number and hash
* Gas usage and roots
* Transaction list (truncated hashes)
* Balance update count
* Receipt summaries

## Running the Listener

1\. Build and Run

```bash
# Build the application
go build -o flashblocks-listener

# Run on mainnet (default)
./flashblocks-listener

# Run on Sepolia testnet
./flashblocks-listener -network=sepolia
```

2. Expected Output

When running, you'll see Flashblocks streaming in real-time:

{% code overflow="wrap" %}
```bash
2026/01/23 12:06:55 Connecting to Base flashblocks on mainnet: wss://mainnet.flashblocks.base.org/ws
2026/01/23 12:06:57 Connected! Listening for flashblocks...
═══════════════════════════════════════════════════════════════
FLASHBLOCK #3 | Block: 41188536 | Hash: 0x1832098f...7ebd4a7f
═══════════════════════════════════════════════════════════════
  Gas Used:       0x124f501
  Blob Gas Used:  0x22e3d6
  State Root:     0x00000000...00000000
  Receipts Root:  0xa83fdcff...1e433d49

  Transactions: 16
    [0] 0x02f90133...60eff264...
    [1] 0x02f90133...4732f1c0...
    [2] 0x02f90133...01fd3ba7...
    [3] 0x02f90133...470392b3...
    [4] 0x02f90196...e4134811...
    [5] 0x02f901f3...0e62c9ff...
    [6] 0x02f90153...b0328e40...
    [7] 0x02f90153...f04b2f4f...
    [8] 0x02f8c082...7ceecf2b...
    [9] 0x02f90153...1f75269c...
    [10] 0x02f90151...c9b5718a...
    [11] 0xf86c822a...3bd176a3...
    [12] 0xf9014183...06eeeac4...
    [13] 0x02f88482...1d4b11e4...
    [14] 0x02f88482...97783f64...
    [15] 0x02f90354...def1bfb6...

  Account Balance Updates: 205

  Receipts: 16
    [EIP-1559] 0x7c69632d...c315f13a - Status: 0x1, Logs: 0
    [EIP-1559] 0xda9e6136...1fdde572 - Status: 0x1, Logs: 0
    [EIP-1559] 0xe2030a9f...399a02ce - Status: 0x1, Logs: 0
    ... and 13 more receipts
```
{% endcode %}

You'll see new Flashblocks appear approximately every 200 milliseconds.

3. Shutting Down

Press `Ctrl+C` to stop the listener. It will send a proper WebSocket close message before exiting:

```bash
^C2026/01/23 12:06:59 Received signal interrupt, shutting down...
```

## Extending for Trading Applications

Now that you have the basic listener working, here are some ways to extend it for trading:

### 1. Filter Transactions by Address

Monitor specific addresses (your wallet, a DEX router, etc.):

```go
func filterByAddress(fb *Flashblock, targetAddress string) {
    for txHash, receipt := range fb.Metadata.Receipts {
        data := receipt.GetData()
        if data == nil {
            continue
        }
        for _, logEntry := range data.Logs {
            if strings.EqualFold(logEntry.Address, targetAddress) {
                fmt.Printf("Activity detected! TX: %s\n", txHash)
                // Process the event...
            }
        }
    }
}
```

### 2. Detect Swap Events

Watch for Uniswap V2/V3 swap events by checking the first topic (event signature):

```go
const (
    // Uniswap V2 Swap event signature
    uniswapV2Swap = "0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822"
    // Uniswap V3 Swap event signature  
    uniswapV3Swap = "0xc42079f94a6350d7e6235f29174924f928cc2ac818eb64fed8004e115fbcca67"
)

func detectSwaps(fb *Flashblock) {
    for txHash, receipt := range fb.Metadata.Receipts {
        data := receipt.GetData()
        if data == nil {
            continue
        }
        for _, logEntry := range data.Logs {
            if len(logEntry.Topics) > 0 {
                switch logEntry.Topics[0] {
                case uniswapV2Swap:
                    fmt.Printf("Uniswap V2 Swap in TX: %s\n", txHash)
                case uniswapV3Swap:
                    fmt.Printf("Uniswap V3 Swap in TX: %s\n", txHash)
                }
            }
        }
    }
}
```

### 3. Track Balance Changes

Monitor when specific addresses receive or send ETH:

```go
func trackBalanceChanges(fb *Flashblock, watchAddresses []string) {
    for _, addr := range watchAddresses {
        if newBalance, exists := fb.Metadata.NewAccountBalances[addr]; exists {
            fmt.Printf("Balance update for %s: %s\n", addr, newBalance)
        }
    }
}
```

## Important Considerations

1. **Preconfirmation vs. Finality**

Flashblocks provide **preconfirmations**, not final confirmations. While extremely reliable, there's a small chance (during rare reorgs) that a Flashblock's data might differ from the final block.

For critical transactions:

* Use Flashblocks for fast UI feedback
* Wait for full block confirmation for settlement

2. **Rate Limits**

The public endpoints (`wss://mainnet.flashblocks.base.org/ws`) are rate-limited. For production applications:

* Use a node provider ([GetBlock](https://account.getblock.io)) with Flashblocks support
* Consider running your own Flashblocks-aware node

3. **Handling Disconnections**

In production, add reconnection logic:

{% code overflow="wrap" %}
```go
func connectWithRetry(wsURL string, maxRetries int) (*websocket.Conn, error) {
    for i := 0; i < maxRetries; i++ {
        conn, _, err := websocket.DefaultDialer.Dial(wsURL, nil)
        if err == nil {
            return conn, nil
        }
        log.Printf("Connection attempt %d failed: %v", i+1, err)
        time.Sleep(time.Second * time.Duration(i+1))
    }
    return nil, fmt.Errorf("failed after %d attempts", maxRetries)
}
```
{% endcode %}

## Complete Code

Here's the full `main.go` file:

<details>

<summary>Full Code</summary>

{% code title="main.go" overflow="wrap" %}
```go
package main

import (
	"bytes"
	"encoding/json"
	"flag"
	"fmt"
	"io"
	"log"
	"os"
	"os/signal"
	"syscall"

	"github.com/andybalholm/brotli"
	"github.com/gorilla/websocket"
)

const (
	mainnetWSURL = "wss://mainnet.flashblocks.base.org/ws"
	sepoliaWSURL = "wss://sepolia.flashblocks.base.org/ws"
)

// Flashblock represents the root structure of a flashblock message
type Flashblock struct {
	Diff     BlockDiff `json:"diff"`
	Index    int       `json:"index"`
	Metadata Metadata  `json:"metadata"`
}

// BlockDiff contains the block differences/updates
type BlockDiff struct {
	BlobGasUsed     string   `json:"blob_gas_used"`
	BlockHash       string   `json:"block_hash"`
	GasUsed         string   `json:"gas_used"`
	LogsBloom       string   `json:"logs_bloom"`
	ReceiptsRoot    string   `json:"receipts_root"`
	StateRoot       string   `json:"state_root"`
	Transactions    []string `json:"transactions"`
	Withdrawals     []any    `json:"withdrawals"`
	WithdrawalsRoot string   `json:"withdrawals_root"`
}

// Metadata contains block metadata including balances and receipts
type Metadata struct {
	BlockNumber        uint64             `json:"block_number"`
	NewAccountBalances map[string]string  `json:"new_account_balances"`
	Receipts           map[string]Receipt `json:"receipts"`
}

// Receipt represents a transaction receipt (can be EIP-1559 or Legacy)
type Receipt struct {
	Eip1559 *ReceiptData `json:"Eip1559,omitempty"`
	Legacy  *ReceiptData `json:"Legacy,omitempty"`
}

// GetData returns the receipt data regardless of type
func (r *Receipt) GetData() *ReceiptData {
	if r.Eip1559 != nil {
		return r.Eip1559
	}
	return r.Legacy
}

// GetType returns the receipt type as a string
func (r *Receipt) GetType() string {
	if r.Eip1559 != nil {
		return "EIP-1559"
	}
	if r.Legacy != nil {
		return "Legacy"
	}
	return "Unknown"
}

// ReceiptData contains the actual receipt information
type ReceiptData struct {
	CumulativeGasUsed string `json:"cumulativeGasUsed"`
	Logs              []Log  `json:"logs"`
	Status            string `json:"status"`
}

// Log represents an event log
type Log struct {
	Address string   `json:"address"`
	Data    string   `json:"data"`
	Topics  []string `json:"topics"`
}

func main() {
	network := flag.String("network", "mainnet", "Network to connect to: mainnet or sepolia")
	flag.Parse()

	var wsURL string
	switch *network {
	case "mainnet":
		wsURL = mainnetWSURL
	case "sepolia":
		wsURL = sepoliaWSURL
	default:
		log.Fatalf("Invalid network: %s. Use 'mainnet' or 'sepolia'", *network)
	}

	log.Printf("Connecting to Base flashblocks on %s: %s", *network, wsURL)

	conn, _, err := websocket.DefaultDialer.Dial(wsURL, nil)
	if err != nil {
		log.Fatalf("Failed to connect to WebSocket: %v", err)
	}
	defer conn.Close()

	log.Println("Connected! Listening for flashblocks...")

	// Handle graceful shutdown
	sigChan := make(chan os.Signal, 1)
	signal.Notify(sigChan, syscall.SIGINT, syscall.SIGTERM)

	done := make(chan struct{})

	go func() {
		defer close(done)
		for {
			messageType, message, err := conn.ReadMessage()
			if err != nil {
				if websocket.IsCloseError(err, websocket.CloseNormalClosure, websocket.CloseGoingAway) {
					log.Println("WebSocket closed normally")
					return
				}
				log.Printf("Error reading message: %v", err)
				return
			}

			switch messageType {
			case websocket.TextMessage:
				handleFlashblockJSON(message)
			case websocket.BinaryMessage:
				decoded, err := decodeBrotli(message)
				if err != nil {
					log.Printf("Error decoding Brotli: %v", err)
					log.Printf("Raw binary message length: %d bytes", len(message))
					continue
				}
				handleFlashblockJSON(decoded)
			default:
				log.Printf("Received unknown message type: %d", messageType)
			}
		}
	}()

	select {
	case <-done:
		log.Println("Connection closed")
	case sig := <-sigChan:
		log.Printf("Received signal %v, shutting down...", sig)
		err := conn.WriteMessage(websocket.CloseMessage, websocket.FormatCloseMessage(websocket.CloseNormalClosure, ""))
		if err != nil {
			log.Printf("Error sending close message: %v", err)
		}
	}
}

func decodeBrotli(data []byte) ([]byte, error) {
	reader := brotli.NewReader(bytes.NewReader(data))
	return io.ReadAll(reader)
}

func handleFlashblockJSON(data []byte) {
	var flashblock Flashblock
	if err := json.Unmarshal(data, &flashblock); err != nil {
		log.Printf("Error parsing flashblock JSON: %v", err)
		log.Printf("Raw data: %s", string(data))
		return
	}

	printFlashblock(&flashblock)
}

func printFlashblock(fb *Flashblock) {
	fmt.Println("═══════════════════════════════════════════════════════════════")
	fmt.Printf("FLASHBLOCK #%d | Block: %d | Hash: %s\n",
		fb.Index,
		fb.Metadata.BlockNumber,
		truncateHash(fb.Diff.BlockHash))
	fmt.Println("═══════════════════════════════════════════════════════════════")

	fmt.Printf("  Gas Used:       %s\n", fb.Diff.GasUsed)
	fmt.Printf("  Blob Gas Used:  %s\n", fb.Diff.BlobGasUsed)
	fmt.Printf("  State Root:     %s\n", truncateHash(fb.Diff.StateRoot))
	fmt.Printf("  Receipts Root:  %s\n", truncateHash(fb.Diff.ReceiptsRoot))

	fmt.Printf("\n  Transactions: %d\n", len(fb.Diff.Transactions))
	for i, tx := range fb.Diff.Transactions {
		fmt.Printf("    [%d] %s...\n", i, truncateHash(tx))
	}

	fmt.Printf("\n  Account Balance Updates: %d\n", len(fb.Metadata.NewAccountBalances))

	fmt.Printf("\n  Receipts: %d\n", len(fb.Metadata.Receipts))
	receiptCount := 0
	for txHash, receipt := range fb.Metadata.Receipts {
		if receiptCount >= 3 {
			fmt.Printf("    ... and %d more receipts\n", len(fb.Metadata.Receipts)-3)
			break
		}
		data := receipt.GetData()
		if data != nil {
			fmt.Printf("    [%s] %s - Status: %s, Logs: %d\n",
				receipt.GetType(),
				truncateHash(txHash),
				data.Status,
				len(data.Logs))
		}
		receiptCount++
	}

	fmt.Println()
}

func truncateHash(hash string) string {
	if len(hash) <= 20 {
		return hash
	}
	return hash[:10] + "..." + hash[len(hash)-8:]
}
```
{% endcode %}

</details>

## Conclusion

You've built a real-time Base Flashblocks listener in Go that:

* Connects to the Flashblocks WebSocket stream
* Handles Brotli-compressed messages
* Parses and displays transaction data, receipts, and balance updates

### Resources

* [Base Flashblocks Documentation](https://docs.base.org/chain/flashblocks)
* [Base Blog: Flashblocks Deep Dive](https://blog.base.dev/flashblocks-deep-dive)
* [Base Flashblocks Listener Repo](https://github.com/GetBlock-io/guides/tree/main/base-flashblocks-listener)
* [Flashbots Documentation](https://docs.flashbots.net/)
* [GetBlock Dashboard](https://account.getblock.io/)
