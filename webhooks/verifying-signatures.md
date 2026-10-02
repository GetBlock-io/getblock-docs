---
description: >-
  Verify the X-GetBlock-Signature header of every webhook delivery, with
  ready-to-use Go, JavaScript and Python recipes, a published test vector and
  the secret rotation procedure.
---

# Verifying Signatures

Every delivery carries an `X-GetBlock-Signature` header: an HMAC-SHA256 of the request body made with the webhook's signing secret (`whsec_…`). Verify it on every request — it proves the request came from GetBlock and that the body was not changed.

### Signature scheme

```text
X-GetBlock-Signature: t=<unix seconds>,v1=<hex>[,v1=<hex>]
v1 = hex( HMAC-SHA256( secret, "<t>" + "." + body ) )
```

`t` is the time the attempt was signed. There is one `v1` per valid secret: one normally, two during a secret rotation.

### Verification steps

1. **Use the raw body bytes** exactly as received, before any JSON parsing. A framework that parses and re-serialises the body changes the bytes and breaks the signature.
2. **Parse the header.** Elements are comma-separated `key=value`; spaces around an element are allowed. Expect exactly one `t` (decimal digits) and at least one `v1` (64 hex digits, either case). Ignore unknown keys such as `v2=…` — the scheme may gain new ones. A second `t`, a missing or non-numeric `t`, no `v1`, a `v1` that is not 64 hex digits, or an element without `=` is a malformed header.
3. **Check the time first:** reject the request when `|now − t|` is more than 300 seconds, in either direction. A clock problem on your side then shows as *expired*, not as a forged body.
4. **Compute the MAC** for every secret you accept and compare it with every `v1` in constant time. **Any match is success.** During a rotation the header carries two `v1` values — do not rely on their order.
5. **Header names are case-insensitive:** many frameworks hand this one over as `X-Getblock-Signature`.

Every attempt, retries included, is signed afresh with a new `t`. A request repeated within the 300 seconds cannot be told apart from a legitimate repeat of an at-least-once delivery, so de-duplicate on `eventId` + phase — see [Handling Chain Reorganizations](handling-chain-reorganizations.md#at-least-once-delivery).

{% hint style="warning" %}
Do not rely on IP addresses to recognise GetBlock: the delivery addresses are not published and may change without notice. The signature is the only reliable check of origin. See [Delivery source addresses](retries-and-endpoint-protection.md#delivery-source-addresses).
{% endhint %}

### Recipes

Each recipe is a complete program that uses only the standard library. The `verifySignature` / `verify_signature` function is what you copy into your handler, or import from the saved file as in [Getting Started](getting-started.md). The `main` function reads one JSON object from stdin — `header`, the body as `body` (text) or `body_b64` (base64), `secrets`, and optional `now` and `tolerance` in seconds — and prints `ok`, `malformed`, `expired`, `mismatch` or `no_secrets`.

Where the raw body is in common frameworks: Go `io.ReadAll(r.Body)`; Express `express.raw({ type: "application/json" })`, then `req.body` is a `Buffer`; Flask `request.get_data()`; FastAPI `await request.body()`; Django `request.body`.

{% tabs %}
{% tab title="Go" %}
Go 1.21 or later. Save as `verify.go`:

{% code overflow="wrap" %}
```go
// verify.go — GetBlock Notify webhook signature check (standard library only).
package main

import (
	"crypto/hmac"
	"crypto/sha256"
	"encoding/base64"
	"encoding/hex"
	"encoding/json"
	"errors"
	"fmt"
	"os"
	"strconv"
	"strings"
	"time"
)

var (
	ErrNoSecrets = errors.New("no_secrets")
	ErrMalformed = errors.New("malformed")
	ErrExpired   = errors.New("expired")
	ErrMismatch  = errors.New("mismatch")
)

// verifySignature checks an X-GetBlock-Signature header against the raw body.
// secrets are all the secrets you accept right now: the current one and,
// during a rotation overlap, the previous one.
func verifySignature(header string, body []byte, secrets []string, now time.Time, tolerance time.Duration) error {
	var keys [][]byte
	for _, s := range secrets {
		if s != "" {
			keys = append(keys, []byte(s))
		}
	}
	if len(keys) == 0 {
		return ErrNoSecrets
	}

	var t int64
	seenT := false
	var macs [][]byte
	for _, element := range strings.Split(header, ",") {
		key, value, ok := strings.Cut(strings.TrimSpace(element), "=")
		if !ok {
			return ErrMalformed
		}
		switch key {
		case "t":
			if seenT || !isDigits(value) {
				return ErrMalformed
			}
			n, err := strconv.ParseInt(value, 10, 64)
			if err != nil {
				return ErrMalformed
			}
			t, seenT = n, true
		case "v1":
			mac, err := hex.DecodeString(value)
			if err != nil || len(mac) != sha256.Size {
				return ErrMalformed
			}
			macs = append(macs, mac)
		}
		// Unknown keys (v2=...) are ignored on purpose.
	}
	if !seenT || len(macs) == 0 {
		return ErrMalformed
	}

	skew := now.Unix() - t
	if skew < 0 {
		skew = -skew
	}
	if skew > int64(tolerance/time.Second) {
		return ErrExpired
	}

	for _, key := range keys {
		m := hmac.New(sha256.New, key)
		m.Write([]byte(strconv.FormatInt(t, 10) + "."))
		m.Write(body)
		expected := m.Sum(nil)
		for _, mac := range macs {
			if hmac.Equal(expected, mac) {
				return nil
			}
		}
	}

	return ErrMismatch
}

func isDigits(s string) bool {
	if s == "" {
		return false
	}
	for i := 0; i < len(s); i++ {
		if s[i] < '0' || s[i] > '9' {
			return false
		}
	}

	return true
}

func main() {
	var in struct {
		Header    string   `json:"header"`
		Body      *string  `json:"body"`
		BodyB64   string   `json:"body_b64"`
		Secrets   []string `json:"secrets"`
		Now       *int64   `json:"now"`
		Tolerance *int64   `json:"tolerance"`
	}
	if err := json.NewDecoder(os.Stdin).Decode(&in); err != nil {
		fmt.Fprintln(os.Stderr, "bad input:", err)
		os.Exit(2)
	}

	var body []byte
	if in.Body != nil {
		body = []byte(*in.Body)
	} else {
		b, err := base64.StdEncoding.DecodeString(in.BodyB64)
		if err != nil {
			fmt.Fprintln(os.Stderr, "bad body_b64:", err)
			os.Exit(2)
		}
		body = b
	}

	now := time.Now()
	if in.Now != nil {
		now = time.Unix(*in.Now, 0)
	}
	tolerance := 300 * time.Second
	if in.Tolerance != nil {
		tolerance = time.Duration(*in.Tolerance) * time.Second
	}

	if err := verifySignature(in.Header, body, in.Secrets, now, tolerance); err != nil {
		fmt.Println(err)
		os.Exit(1)
	}
	fmt.Println("ok")
}
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
Node.js 18 or later. Save as `verify.mjs`:

{% code overflow="wrap" %}
```javascript
// verify.mjs — GetBlock Notify webhook signature check (no dependencies).
import { createHmac, timingSafeEqual } from "node:crypto";
import { pathToFileURL } from "node:url";

const HEX64 = /^[0-9a-fA-F]{64}$/;
const DIGITS = /^[0-9]+$/;

// verifySignature checks an X-GetBlock-Signature header against the raw body
// (a Buffer). secrets are all the secrets you accept right now: the current
// one and, during a rotation overlap, the previous one.
export function verifySignature(header, body, secrets, now = Date.now() / 1000, tolerance = 300) {
  const keys = (secrets || []).filter((s) => typeof s === "string" && s !== "");
  if (keys.length === 0) return { ok: false, reason: "no_secrets" };

  let t = null;
  const macs = [];
  for (const element of String(header).split(",")) {
    const item = element.trim();
    const eq = item.indexOf("=");
    if (eq < 0) return { ok: false, reason: "malformed" };
    const key = item.slice(0, eq);
    const value = item.slice(eq + 1);
    if (key === "t") {
      if (t !== null || !DIGITS.test(value)) return { ok: false, reason: "malformed" };
      t = Number(value);
      if (!Number.isSafeInteger(t)) return { ok: false, reason: "malformed" };
    } else if (key === "v1") {
      if (!HEX64.test(value)) return { ok: false, reason: "malformed" };
      macs.push(Buffer.from(value, "hex"));
    }
    // Unknown keys (v2=...) are ignored on purpose.
  }
  if (t === null || macs.length === 0) return { ok: false, reason: "malformed" };

  if (Math.abs(Math.floor(now) - t) > tolerance) return { ok: false, reason: "expired" };

  for (const key of keys) {
    const expected = createHmac("sha256", key).update(String(t) + ".").update(body).digest();
    if (macs.some((mac) => timingSafeEqual(expected, mac))) return { ok: true, reason: "ok" };
  }
  return { ok: false, reason: "mismatch" };
}

async function main() {
  const chunks = [];
  for await (const chunk of process.stdin) chunks.push(chunk);
  const input = JSON.parse(Buffer.concat(chunks).toString("utf8"));
  const body = input.body !== undefined
    ? Buffer.from(input.body, "utf8")
    : Buffer.from(input.body_b64 || "", "base64");
  const result = verifySignature(
    input.header || "",
    body,
    input.secrets || [],
    input.now !== undefined ? input.now : Date.now() / 1000,
    input.tolerance !== undefined ? input.tolerance : 300,
  );
  console.log(result.reason);
  process.exitCode = result.ok ? 0 : 1;
}

if (process.argv[1] && import.meta.url === pathToFileURL(process.argv[1]).href) {
  await main();
}
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
Python 3.9 or later. Save as `verify.py`:

{% code overflow="wrap" %}
```python
#!/usr/bin/env python3
"""verify.py — GetBlock Notify webhook signature check (standard library only)."""
import base64
import hashlib
import hmac
import json
import re
import sys
import time

_HEX64 = re.compile(r"[0-9a-fA-F]{64}")
_DIGITS = re.compile(r"[0-9]+")


def verify_signature(header, body, secrets, now=None, tolerance=300):
    """Check an X-GetBlock-Signature header against the raw body (bytes).

    secrets are all the secrets you accept right now: the current one and,
    during a rotation overlap, the previous one. Returns (ok, reason).
    """
    keys = [s.encode("utf-8") for s in secrets if s]
    if not keys:
        return False, "no_secrets"

    t = None
    macs = []
    for element in header.split(","):
        key, sep, value = element.strip().partition("=")
        if not sep:
            return False, "malformed"
        if key == "t":
            if t is not None or not _DIGITS.fullmatch(value):
                return False, "malformed"
            t = int(value)
        elif key == "v1":
            if not _HEX64.fullmatch(value):
                return False, "malformed"
            macs.append(bytes.fromhex(value))
        # Unknown keys (v2=...) are ignored on purpose.
    if t is None or not macs:
        return False, "malformed"

    if now is None:
        now = time.time()
    if abs(int(now) - t) > tolerance:
        return False, "expired"

    signed = str(t).encode("ascii") + b"." + body
    for key in keys:
        expected = hmac.new(key, signed, hashlib.sha256).digest()
        if any(hmac.compare_digest(expected, mac) for mac in macs):
            return True, "ok"
    return False, "mismatch"


def main():
    data = json.load(sys.stdin)
    if "body" in data:
        body = data["body"].encode("utf-8")
    else:
        body = base64.b64decode(data.get("body_b64", ""))
    ok, reason = verify_signature(
        data.get("header", ""),
        body,
        data.get("secrets", []),
        data.get("now"),
        data.get("tolerance", 300),
    )
    print(reason)
    sys.exit(0 if ok else 1)


if __name__ == "__main__":
    main()
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Self-test with the test vector

Every GetBlock signer and every recipe above reproduces this vector byte for byte. The secrets are fake and are never used in production. `body` is the confirmed-phase `log_event` from [Delivery Format](delivery-format.md#example), compact, stored as a JSON string so no editor can change its bytes. Save it as `signature_v1.json`:

{% code overflow="wrap" %}
```json
{
  "secret": "whsec_GetB1ockNotifyGo1denVectorSecretDoNotUseProd",
  "previous_secret": "whsec_PreviousGo1denVectorSecretDoNotUseProdSecond",
  "t": 1789455600,
  "body": "{\"webhookId\":\"wh_9f0c2d7e\",\"eventId\":\"eth:mainnet:receipts:0x3805a4c919e7023ab2dc9be9b9bdf2bc44e0c157028948967c5f5f6f227baa64:0x1b6e7b7715daac0582a3039eed9b453d6bf520765b9dc4da77a4a83d457c4083:0\",\"type\":\"log_event\",\"network\":\"eth-mainnet\",\"confirmed\":true,\"removed\":false,\"block\":{\"number\":\"0xac9f38\",\"hash\":\"0x3805a4c919e7023ab2dc9be9b9bdf2bc44e0c157028948967c5f5f6f227baa64\",\"timestamp\":\"0x6a5e1ef4\"},\"raw\":{\"address\":\"0x5b6fade3b2d59d01d4f996aed530332165bb391e\",\"topics\":[\"0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef\",\"0x000000000000000000000000691f834aeeb8167551a94d933fc6a66011f38ad3\",\"0x0000000000000000000000002f1528f344f6410d361654d22a47ef121a60c938\"],\"data\":\"0x00000000000000000000000000000000000000000000000135dd58a691664000\",\"blockNumber\":\"0xac9f38\",\"transactionHash\":\"0x1b6e7b7715daac0582a3039eed9b453d6bf520765b9dc4da77a4a83d457c4083\",\"transactionIndex\":\"0x0\",\"blockHash\":\"0x3805a4c919e7023ab2dc9be9b9bdf2bc44e0c157028948967c5f5f6f227baa64\",\"blockTimestamp\":\"0x6a5e1ef4\",\"logIndex\":\"0x0\",\"removed\":false},\"decoded\":{\"event\":\"Transfer\",\"signature\":\"Transfer(address,address,uint256)\",\"params\":{\"from\":\"0x691f834aeeb8167551a94d933fc6a66011f38ad3\",\"to\":\"0x2f1528f344f6410d361654d22a47ef121a60c938\",\"value\":\"0x135dd58a691664000\"}}}",
  "header": "t=1789455600,v1=663d17a7e5aa07ca7df21050f7688cc54a78e4fa9cfbb5d1774e4df5e31822d4",
  "header_with_previous": "t=1789455600,v1=663d17a7e5aa07ca7df21050f7688cc54a78e4fa9cfbb5d1774e4df5e31822d4,v1=9bb2ef4f8c5709f8e526367b24fe4ac21e728ba711fdc033979ad4e998c1c266"
}
```
{% endcode %}

Run each recipe against it (`jq` builds the stdin object); the expected output is at the end of each line:

{% code overflow="wrap" %}
```sh
jq -c '{header, body, secrets: [.secret], now: .t}' signature_v1.json | go run verify.go        # ok
jq -c '{header: .header_with_previous, body, secrets: [.previous_secret], now: .t}' signature_v1.json | node verify.mjs  # ok
jq -c '{header, body, secrets: ["whsec_wrong"], now: .t}' signature_v1.json | python3 verify.py  # mismatch
jq -c '{header, body, secrets: [.secret], now: (.t + 301)}' signature_v1.json | python3 verify.py  # expired
```
{% endcode %}

### Rotating the secret

The secret is shown once: in the answer that creates the webhook and in each rotation answer. Rotate it with **Rotate secret** in the dashboard or with `POST /api/v1/webhooks/{id}/secret/rotate`:

| Request body | Effect | `previous_expires_at` |
| --- | --- | --- |
| none, `{}` or `{"expire_previous": false}` | **Routine:** the old secret stays valid for **24 hours**; meanwhile every delivery carries two `v1` values, one per secret | The end of the overlap |
| `{"expire_previous": true}` | **Emergency** (the secret has leaked): the old secret stops at once. In the dashboard: **Rotate and revoke old secret** | `null` |

The answer is `200` with the new secret: `{"secret": "whsec_…", "version": 4, "previous_expires_at": "2026-09-26T10:00:00Z"}`. In the dashboard the new secret is shown once in the **Your new signing secret** window.

For a routine rotation:

1. Rotate and add the new secret to your verifier, next to the old one.
2. Keep accepting both until `previous_expires_at`.
3. Drop the old secret.

There is only one previous secret: a second rotation inside the 24 hours retires the older one at once. Deliveries may keep using the previous set of secrets for up to 60 seconds after a rotation, which the overlap covers.
