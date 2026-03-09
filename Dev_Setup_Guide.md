# Dev Setup Guide

> Get everything in this file sorted BEFORE writing code. API keys, testnet addresses, auth patterns, and critical gotchas -- all in one place.

---

## Section 1: API Keys and Credentials

**Get these before the hackathon.** DeFindex and Etherfuse have no self-service portal -- you have to contact the team. Budget 24-48h for responses.

| Protocol | Key Required | How to Get | Wait | Contact |
|---|---|---|---|---|
| Etherfuse | Yes | Discord / email | 24-48h | stablebond@etherfuse.com |
| DeFindex | Yes | Discord DM | 24-48h | Discord: paltalabs |
| Soroswap | No | -- | -- | -- |
| Phoenix | No | -- | -- | -- |
| Aquarius | No | -- | -- | -- |
| Blend | No | -- | -- | -- |
| Reflector Oracle | TBD | TBD | TBD | TBD |
| BlindPay | TBD | TBD | TBD | TBD |
| AlfredPay | TBD | TBD | TBD | TBD |

---

### Etherfuse

- **Sandbox base URL:** `https://api.sand.etherfuse.com`
- **Production base URL:** `https://api.etherfuse.com`
- **Validate your key:** `GET /ramp/me` returns org name + KYB timestamp
- **Auth header:** `Authorization: your-api-key` -- NO "Bearer" prefix (see Section 4)
- **customer_id:** UUID you generate yourself, store permanently, reuse forever (see Section 5)
- **bankAccountId:** same pattern -- you assign it, Etherfuse binds it on first use
- Sandbox auto-approves bank accounts; production requires KYB review

### DeFindex

- **SDK env var:** `DEFINDEX_API_KEY`
- **Auth:** `Authorization: Bearer your-api-key` -- standard Bearer, opposite of Etherfuse
- No docs page for self-service signup (returns 404); Discord is the only path

> [TEAM: Add self-service API key generation or at least a signup form for DeFindex.]

**Flagged empty -- team to fill:**
- BlindPay sandbox access process
- AlfredPay sandbox access process
- Reflector Oracle auth requirements

---

## Section 2: Testnet Contract Addresses

These are the actual deployed addresses on Stellar testnet. Testnet contracts get redeployed periodically -- if something stops working, check the protocol's GitHub for updated addresses.

### Stellar Testnet Infrastructure

| Resource | Value |
|---|---|
| Soroban RPC | `https://soroban-testnet.stellar.org` |
| Horizon | `https://horizon-testnet.stellar.org` |
| Friendbot | `https://friendbot.stellar.org` |

---

### DeFindex (testnet)

Verified from paltalabs registry, DevRel experiment Run 2.

| Contract | Address | Verified |
|---|---|---|
| Factory | `CDSCWE4GLNBYYTES2OCYDFQA2LLY4RBIAX6ZI32VSUXD7GO6HRPO4A32` | Yes |
| USDC vault | `CBMVK2JK6NTOT2O4HNQAIQFJY232BHKGLIMXDVQVHIIZKDACXDFZDWHN` | Yes |
| XLM vault | `CCLV4H7WTLJQ7ATLHBBQV2WW3OINF3FOY5XZ7VPHZO7NH3D2ZS4GFSF6` | Yes |

**Verification note:** Run 1 (wrong addresses) got `Error(Storage, MissingValue)`. Run 2 (paltalabs addresses above) got `Error(WasmVm, InvalidAction)` -- meaning the contract IS deployed and running; the error was a call-level issue, not an address issue.

---

### Soroswap (testnet)

| Contract | Address |
|---|---|
| Router / Aggregator | TBD |
| Factory | TBD |

> [TEAM: Provide verified testnet addresses for Soroswap.]

---

### Phoenix (testnet)

| Contract | Address |
|---|---|
| Factory | TBD |
| Router | TBD |

> [TEAM: Provide verified testnet addresses for Phoenix.]

---

### Blend (testnet)

| Contract | Address |
|---|---|
| Pool Factory | TBD |
| USDC Pool | TBD |

> [TEAM: Provide verified testnet addresses for Blend.]

---

### Aquarius (testnet)

| Contract | Address |
|---|---|
| AMM Contract | TBD |

> [TEAM: Provide verified testnet address for Aquarius AMM.]

---

### Reflector Oracle (testnet)

| Contract | Address |
|---|---|
| Price Oracle | TBD |

> [TEAM: Provide verified testnet address for Reflector Oracle.]

---

> [TEAM: Maintain a single JSON file in a public repo with all verified testnet addresses across all protocols. Missing or wrong addresses were the #1 time sink in hackathon Run 1 -- participants spent 90+ minutes discovering the addresses before writing a single line of business logic.]

---

## Section 3: Testnet Asset Registry

**The single most expensive gotcha (90+ min in experiments):** testnet USDC has multiple issuers that do NOT share liquidity. Pick the wrong one and swaps silently fail.

### USDC Issuers on Testnet

| Source | Issuer Address | Used by |
|---|---|---|
| Circle (standard testnet) | `GBBD47IF6LWK7P7MDEVSCWR7DPUWV3NY3DTQEVFL4NAT4AQH3ZLLFLA5` | Soroswap, SDEX, most protocols |
| Blend / Etherfuse | `GATALTGTWIOT6BUDBCZM3Q4OQ4BO2COLOAZ7IYSKPLC2PMSOPPGF5V56` | Blend pools, Etherfuse flows |

**Rule:** Check which issuer your protocol expects. Do NOT assume USDC is USDC on testnet.

---

### Etherfuse Assets

**Testnet issuer:** `GC3CW7EDYRTWQ635VDIGY6S4ZUF5L6TQ7AA4MWS7LEQDBLUSZXV7UPS4`
**Mainnet / production issuer:** `GCRYUGD5NVARGXT56XEZI5CIFCQETYHAPQQTHO2O3IQZTHDH4LATMYWC`

| Asset Code | Name | Notes |
|---|---|---|
| CETES | Mexican CETES Bonds | Primary tested asset |
| USTRY | US Treasury Notes | Verified on testnet |
| KTB | Korean Treasury Bonds | Listed in testnet toml |
| CARN | Brazilian Tesouro Bonds | Listed in testnet toml |
| CZERO | CETES Zero (0% APY) | Anchor asset = MXN |

**Testnet limitation:** There is no CETES/USDC liquidity on the testnet SDEX. `pathPaymentStrictSend` will always fail with `op_no_path`. You can test the trustline flow, but not the actual buy/sell swap.

> [TEAM: A testnet CETES faucet, or a funded test account with stablebond balances, would eliminate this blocker entirely. Currently impossible to test end-to-end buy/sell flows on testnet.]

---

## Section 4: Authentication Patterns

Auth format varies per protocol in ways that are not documented anywhere. This table is the reference.

| Protocol | Header Format | Notes |
|---|---|---|
| Etherfuse | `Authorization: your-api-key` | NO "Bearer" prefix. Raw key only. |
| DeFindex | `Authorization: Bearer your-api-key` | Standard Bearer. Opposite of Etherfuse. |
| Soroban RPC | Varies by provider | Some use Bearer, some use `X-API-Key` |
| Horizon | None | IP rate limit: 3,600 req/hour per IP |
| Soroswap | None | No auth required |
| Phoenix | None | No auth required |
| Aquarius | None | No auth required |

### Etherfuse (correct)

```typescript
headers: {
  'Authorization': process.env.ETHERFUSE_API_KEY,
  'Content-Type': 'application/json'
}
```

### DeFindex (correct)

```typescript
headers: {
  'Authorization': `Bearer ${process.env.DEFINDEX_API_KEY}`,
  'Content-Type': 'application/json'
}
```

**Horizon rate limiting note:** Streaming connections count against the 3,600 req/hour quota. If you're polling and streaming simultaneously, budget accordingly.

---

## Section 5: Critical Setup Gotchas

These are the issues that blocked developers for 30-120 minutes each across 60 build runs. Know them upfront.

---

### Etherfuse: customer_id is permanent

Generate exactly once per user, store in your database, reuse forever. If you generate a new UUID per request, you'll get "Bank account not found" errors with no useful debug info. Both `customer_id` and `bankAccountId` follow this pattern -- you assign them, Etherfuse binds them on first use.

---

### Etherfuse: order response fields are inconsistent

- Onramp response shape: `{ onramp: { orderId, depositClabe } }`
- Offramp response shape: `{ offramp: { orderId, withdrawalClabe } }`
- Sometimes flat, sometimes wrapped
- `orderId` vs `order_id` vs `id` all appear across different endpoints

Write a normalizer:

```typescript
// Unwrap envelope
const data = raw.onramp || raw.offramp || raw;
// Normalize ID field
const id = data.orderId || data.id || data.order_id;
```

---

### Etherfuse sandbox: simulate fiat manually

Orders don't progress automatically in sandbox. You must trigger state transitions yourself:

```
POST /ramp/order/fiat_received    -- simulate fiat arriving (onramp)
POST /ramp/order/crypto_received  -- simulate crypto arriving (offramp)
```

---

### Etherfuse: 3-10 second indexing delay

After creating an order, wait 3-10 seconds before querying its status. Immediate queries return 404. This is not an error -- the order is being indexed.

---

### Soroban: always simulate before sending

```typescript
const simulation = await server.simulateTransaction(tx);
const assembled = SorobanRpc.assembleTransaction(tx, simulation);
await server.sendTransaction(assembled);
```

Skipping simulation causes confusing failures. This is different from classic Stellar and easy to miss.

---

### sendTransaction returns PENDING, not success

`sendTransaction` returning without error does NOT mean the transaction succeeded. Poll with a max retry ceiling:

```typescript
for (let i = 0; i < 60; i++) {
  await sleep(1000);
  const result = await server.getTransaction(hash);
  if (result.status !== 'NOT_FOUND') return result;
}
throw new Error('Transaction timeout after 60s');
```

---

### DeFindex: XLM must be SAC-wrapped

Native XLM cannot be deposited directly into DeFindex vaults. It must be wrapped as a Stellar Asset Contract (SAC) token first -- an additional contract call before the deposit. This requirement appears nowhere in the docs.

---

### DeFindex: API quirks

- Endpoint is `/vault/` not `/vaults/` -- 404 if you use the plural
- Query param is `?from=<address>` not `?user=<address>`
- Amount format is always an array: `{"amounts": [1000000]}` not `{"amount": 1000000}`
- Success response is HTTP 201, not 200

---

### Trustlines before assets

Establish trustlines before any operation that will receive an asset. Skipping this produces `op_no_destination` or a silent no-op.

---

### Stellar memo limit: 28 bytes

UUIDs are 36 characters. Strip dashes and truncate:

```typescript
const memo = uuid.replace(/-/g, '').slice(0, 28);
```

---

## Section 6: Known Limitations -- Ask Your DevRel Mentor

These are things that don't work smoothly yet or aren't documented. If you hit any of them during the hackathon, don't spin your wheels -- ask a DevRel mentor directly. They can unblock you faster than debugging alone.

### Things that aren't available yet (ask for help)

- **Etherfuse sandbox key** -- no self-service signup exists yet. If you haven't gotten a key before the hackathon starts, find a DevRel mentor immediately. Don't wait for email -- they can often provision one on the spot.
- **DeFindex API key** -- same situation. No self-service path. If you need one during the event, ask DevRel.
- **Testnet CETES balance** -- there's no faucet for stablebond assets on testnet. You can test trustlines but not actual buy/sell flows. Ask your mentor if there's a funded test account available for the event.
- **Unified testnet address list** -- no single canonical source exists for all protocol contract addresses. The TBD rows in Section 2 of this file are genuinely unknown at time of writing. Ask DevRel for the latest before assuming an address is correct.

### Things the docs don't cover (save yourself the debugging time)

- **DeFindex: XLM SAC wrapping** -- required before vault deposits, not mentioned anywhere in the docs. See Section 5.
- **DeFindex: rate limits** -- undocumented. If you're getting throttled, ask a mentor what the current limits are.
- **DeFindex: webhook retry policy** -- undocumented. If you need reliable webhook delivery for your project, ask before building around it.
- **Etherfuse: quote expiration** -- quotes expire after ~2 minutes. Not documented prominently. Design your flow accordingly.

### Known API quirks (not bugs you can fix, just reality)

- **Etherfuse response envelope inconsistency** -- `orderId` vs `order_id` vs `id` varies by endpoint; onramp/offramp responses are wrapped differently. The normalizer in Section 5 handles this.
- **Phoenix rate limiting returns 403** -- looks like an auth error but it's actually a rate limit. If you get a sudden 403 from Phoenix after things were working, wait and retry.
- **Stellar Wallets Kit v1 vs v2** -- breaking change between versions: instance methods became static methods. If you're copying examples from the web and they don't work, check which version you're running.
